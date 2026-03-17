---
{"dg-publish":true,"permalink":"/personal-vault/data-engineering/spark-optimization-techniques/"}
---


## Spark Optimization Techniques


> [!NOTE] Interview Question
> Q. How would you optimize Slow Spark Job ? 
> 


The approach to spark optimization is a three step process.

1. Measure
2. Identify 
3. Remediate

 **Measure (Establish a Baseline):** "I never start optimizing blindly. The first step is to establish a clear, repeatable baseline. I run the job in a production-like environment and capture key metrics: total runtime, resource utilization (CPU/memory), and I/O metrics. This baseline is what I'll compare against to prove my optimizations are working."


**Identify the Bottleneck (The 'Why'):** "With a baseline established, I dive into the **Spark UI**. This is the single most important tool. I'm not just looking at the total runtime; I'm trying to pinpoint the exact stage or task that's consuming the most time."

- **DAG Visualization:** "I start with the Directed Acyclic Graph (DAG) to understand the job's logic and where shuffles are happening."
- **Stages Tab:** "I look for long-running stages. A long stage often points to either a massive data shuffle or a computationally expensive UDF."
- **Tasks Table:** "Within a long stage, I'll examine the task list. If a few tasks are much slower than the rest, it's a classic sign of **data skew**. If all tasks are uniformly slow, it might be an inefficient UDF, memory pressure causing spills, or an I/O bottleneck."
- **Storage Tab:** "I check if my RDDs or DataFrames are being cached effectively, and look for signs of memory pressure like spilling to disk."
- **Executors Tab:** "I check for garbage collection (GC) time. High GC time is a sign that my executor memory is misconfigured."

 **Remediate (The 'How'):** "Once I've identified the likely bottleneck, I'll apply targeted optimization techniques. I address the biggest bottleneck first, re-measure, and then repeat the process if necessary."



The optimizations can be categorized into three main areas. 


#### Shuffle Optimizations 

This is the main culprit most of the times. We talked about Shuffle earlier. A shuffle is the process of redistributing data across partitions, which is extremely expensive due to network I/O and disk I/O. The goal is to avoid or reduce shuffles.



| Problem Symptom                           | Technique / Solution                                                                                                                                                                                                                                                                                                       | Why it Works                                                                                                                                                                                                                                    |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Data Skew** (a few tasks are very slow) | **Salting:** Add a random key to the skewed join column to distribute the data more evenly. <br> **Databricks AQE:** Enable Adaptive Query Execution. Its skew-join optimization can often handle this automatically by splitting large partitions into smaller ones.                                                      | Salting breaks up the "hot" keys that are causing a single partition to be overloaded. AQE does this dynamically at runtime.                                                                                                                    |
| **Too Many Small Files**                  | **Coalesce / Repartition:** Use coalesce() to reduce the number of partitions (avoids a full shuffle) before writing to disk. Use repartition() if you need to increase partitions or change partitioning keys (incurs a full shuffle). <br> **Delta Lake:** Use OPTIMIZE command to compact small files into larger ones. | Reading thousands of small files has high I/O overhead. Compacting them into fewer, larger files (e.g., 128MB-1GB) makes reads much more efficient.                                                                                             |
| **Inefficient Joins**                     | **Broadcast Hash Join (BHJ):** If one DataFrame is small enough (default < 10MB, configurable via spark.sql.autoBroadcastJoinThreshold), Spark can automatically use a BHJ. You can also force it with a broadcast() hint.                                                                                                 | Avoids a massive shuffle. The small table is copied to every executor, allowing the join to happen locally within each partition of the large table. This is the most efficient join type if applicable.                                        |
| **Too many shuffle partitions**           | **Tune spark.sql.shuffle.partitions:** The default is 200. If you have a small amount of data, this creates too many tiny partitions. If you have terabytes, it may be too few. Adjust based on your data size and cluster resources.                                                                                      | The right number of partitions allows for optimal parallelism without creating excessive overhead. A good rule of thumb is 2-3x the number of cores in your cluster. AQE can also dynamically coalesce shuffle partitions.                      |
| **Re-computing the same data**            | **Caching / Persisting:** Use df.cache() (stores in memory) or df.persist(StorageLevel.MEMORY_AND_DISK) on a DataFrame that is used multiple times in subsequent stages.                                                                                                                                                   | Avoids re-executing the entire upstream DAG to re-compute the data. The results of the transformation are stored, so subsequent actions can read directly from memory/disk. **Crucial for iterative algorithms (ML) and interactive analysis.** |


#####  Skew Handling Techniques 


**Data Skew** is a condition where data is unevenly distributed across partitions. In the context of a join or groupBy operation, this means a few keys are associated with a disproportionately large number of records.


**How to spot it in the Spark UI:**  

You go to the **Stages** tab and find a long-running stage. You click on it to view the tasks. The tell-tale sign is the **Task Summary Metrics** , you'll see the **Max** duration is drastically higher than the 75th percentile (or median). You'll also see a few tasks that are processing far more records (Input Size / Records) than the others. This is your bottleneck.


In-Depth Skew Handling Techniques : 

1. Repartitioning 
2. Salting 
3. Broadcast Join
4. Bucketing 
5. Pre-Filtering Skewed keys
6. Range Partitioning or Custom Partitioners


#### 1. Broadcast Join (The Easiest Win)

**What it is:** A technique that avoids a shuffle altogether by sending one entire DataFrame to every executor, which then performs the join locally against its partitions of the larger DataFrame.

**How it works:**

- **Automatically:** If one DataFrame is smaller than the spark.sql.autoBroadcastJoinThreshold (default 10MB), Spark's Catalyst Optimizer will automatically choose this strategy.
    
- **Manually:** You can force it using a hint: large_df.join(broadcast(small_df), "join_key").
    

**When to use it:**

- Any time you are joining a large DataFrame to a small one. This should be your **first thought** in any join scenario.
    
- It's a skew-handling technique because **even if the large DataFrame has skewed keys, it doesn't matter.** The shuffle, which is the part that gets bottlenecked by skew, is completely avoided.
	

**Pros:**

- Extremely effective and fast.
    
- Simple to implement (often automatic).
    

**Cons / Trade-offs:**

- Strictly limited by the size of the smaller DataFrame. Broadcasting a large DataFrame (e.g., > 1 GB) will cause OutOfMemoryError on the driver or executors.
    
- You must be certain the "small" table will always remain small.

 **When It Becomes Inefficient or Dangerous**

- **Small table is not really small** (e.g., 500MB is not “small” when you have 200 executors)
    
    - 200 × 500MB = 100GB of network broadcast → massive traffic spike
    
- **Broadcast table is wide** (many columns, large row size)
    
- **Key skew in small table** causes a few rows to match billions in the big table → **massive executor memory blow-up**
    
- Your cluster has **1000s of executors** (e.g., HFT-like infra) → data replication overhead explodes
    
- You **cache** the broadcasted small table → eats more memory


#### 2. Salting (The Classic Manual Fix for Large-to-Large Joins)

**What it is:** A technique to break up skewed keys by adding a random "salt" to them, effectively creating new, more evenly distributed keys for the join.

**How it works:**  
Imagine you're joining sales and users on user_id, and user_id = 0 (a guest user) is heavily skewed.

1. **On the large (skewed) DataFrame (sales):**
    
    - Create a new column salt with a random integer between 0 and N-1 (e.g., N=10).
        
    - Create a new join key: salted_join_key = concat(user_id, '_', salt).


```python

from pyspark.sql.functions import concat, lit, col, floor, rand

# N is the salting factor
N = 10 
sales_df = sales_df.withColumn("salt", floor(rand() * N))
sales_df = sales_df.withColumn("salted_join_key", concat(col("user_id"), lit("_"), col("salt")))
```


What is happening ? 

Step 1 : 

| Line                         | Purpose                                                 |
| ---------------------------- | ------------------------------------------------------- |
| `rand()`                     | Generates a random float between 0.0 and 1.0            |
| `rand() * N`                 | Scales that float into range [0.0, N)                   |
| `floor()`                    | Converts float to integer in range [0, N-1]             |
| `salt`                       | Now is an **integer between 0 and N-1**, random per row |
| `concat(user_id, '_', salt)` | Creates salted key like `"0_7"` or `"42_3"`             |
|                              |                                                         |

> This splits skewed keys into N distinct groups.


For example:

- Instead of having 400M rows with `user_id = 0`,
- We now have 40M with `0_0`, 40M with `0_1`, … up to `0_9`


2. **On the other DataFrame (users):**

- You must "explode" the join key to match the salt. Create N rows for each original row, one for each possible salt value (0 to N-1).
    

```python
from pyspark.sql.functions import explode, array, lit

salt_df = spark.range(N).toDF("salt")
users_df = users_df.crossJoin(salt_df) # Explode the keys
users_df = users_df.withColumn("salted_join_key", concat(col("user_id"), lit("_"), col("salt"))).drop("salt")
```

users_df 

| user_id |
| ------- |
| 0       |
| 1       |
| 2       |

salt_df

| salt |
| ---- |
| 0    |
| 1    |
| 2    |
| ...  |
| 9    |

After the cross join, we get:


| user_id | salt |
| ------- | ---- |
| 0       | 0    |
| 0       | 1    |
| ...     | ...  |
| 1       | 0    |
| 1       | 1    |
| ...     | ...  |
This gives us **10 versions of each user_id**, one for each salt.

then we compute this last row : 
```python 

users_df.withColumn("salted_join_key", concat(col("user_id"), lit("_"), col("salt")))

```



| user_id | salted_join_key |
| ------- | --------------- |
| 0       | 0_0             |
| 0       | 0_1             |
| 1       | 1_0             |
| ...     | ...             |
|         |                 |


Now format both sales_df and users_df have same salted_join_key.

3. **Perform the Join:** Join the two DataFrames on salted_join_key. The user_id = 0 workload is now spread across 10 different tasks (0_0, 0_1, 0_2, etc.).

```python
sales_df.join(users_df, on="salted_join_key")

```

This join now works in parallel across:

- `0_0`, `0_1`, ..., `0_9` instead of all traffic piling up under just `0`

**When to use it:**

- For large-to-large joins where a Broadcast Join is not possible.
- When you have a few specific, highly skewed keys.

**Pros:**

- Extremely effective at breaking up even the worst skew.

**Cons / Trade-offs:**

- Adds significant code complexity.
- The "explode" step on the smaller of the two tables increases its size by a factor of N, which consumes more memory.
- **Modern Context:** Databricks **Adaptive Query Execution (AQE)** has a skew-join optimization that can often do this dynamically and more efficiently. You should always mention that you'd rely on AQE first, and use manual salting only if AQE is not sufficient.

From Spark 3.2+ , Standalone Spark clusters or Dataproc ( managed GCP clusters ) support AQE as well.

```python

--conf spark.sql.adaptive.enabled=true
--conf spark.sql.adaptive.skewJoin.enabled=true
--conf spark.sql.adaptive.coalescePartitions.enabled=true
--conf spark.sql.adaptive.localShuffleReader.enabled=true

```


But any day , databricks AQE is more effective , why ? 

|Platform|Supports AQE?|Skew Join Optimization?|Notes|
|---|---|---|---|
|**Databricks**|✅ Enabled by default|✅ Very effective|Deep Delta integration, ZORDER, file metadata|
|**Google Cloud Dataproc**|✅ Yes (Spark 3.0+)|✅ But manual tuning needed|You must enable AQE and tune configs|
|**Apache Spark (open source)**|✅ Spark 3.0+|✅ Limited and less automated|No Delta-specific enhancements|
|**Amazon EMR**|✅ Yes|✅ But weaker than Databricks|Depends on Spark version and setup|


> [!NOTE]  What would you do when your join key is heavily skewed, e.g. `user_id = 0` dominates the data?"
> 
> I'd use **salting** — I’d add a random salt to the large table’s key, and then **explode the small table** to create corresponding salted keys, allowing the join to distribute the skewed data over multiple keys and improve parallelism. If the platform supports AQE and skew-join optimization (e.g., Databricks), I’d try that first — but salting is my manual fallback."
> 
> 



#### 3. Pre-Filtering Skewed Keys (The "Divide and Conquer" Strategy)

**What it is:** You isolate the skewed keys, handle them in a separate (often broadcast) join, and then union the result back with the join of the non-skewed keys.

**How it works:**

1. **Identify Skewed Keys:** Run an aggregation to find the "hot" keys.
    
    ```python
    skewed_keys = sales_df.groupBy("user_id").count().orderBy(col("count").desc()).limit(10)
    ```
    

    
2. **Split DataFrames:**
    
    - Create skewed_sales_df containing only records with the hot keys.
        
    - Create normal_sales_df containing all other records.
        
3. **Join Separately:**
    
    - **Normal Join:** Join normal_sales_df with the users table. This should now be a balanced, efficient join.
        
    - **Skewed Join:** The users data corresponding to the skewed keys might now be small enough to broadcast! Join skewed_sales_df with broadcast(skewed_users_df).
        
4. **Union Results:** Use unionByName() to combine the results of the two joins.
    

**When to use it:**

- When the number of distinct skewed keys is small and known.
    
- When the data for the skewed keys on the non-skewed table is small enough to be broadcasted.
    

**Pros:**

- Very targeted and can be highly efficient.
    
- Avoids the complexity of salting.

**Cons / Trade-offs:**

- Requires an extra pass over the data to identify the skewed keys.
    
- Adds significant logical complexity to the pipeline (splitting, joining, unioning).


#### 4. Repartitioning

**What it is:** Redistributing data across a different number of partitions, optionally based on a new set of columns.

**How it works:** df.repartition(200, "join_key")

**When to use it (for skew):**

- This is generally a **blunt and less effective** tool for handling key-based skew. If one key has a billion records, repartitioning won't change the fact that all billion records will still go to the same partition after the shuffle.
    
- It's more useful for fixing skew caused by the input source (e.g., one input file is massive). Repartitioning can help break up that initial large partition before a join.
    

**Pros:**

- Simple syntax.
    

**Cons / Trade-offs:**

- Ineffective for solving key-based skew in joins/aggregations.
    
- Incurs a full, expensive shuffle.


#### 5. Bucketing (A Proactive, Physical Layout Strategy)

**What it is:** A physical data layout strategy where you pre-shuffle and sort data into a fixed number of "buckets" when you write a table.

**How it works:**

Generated python

```python
df.write.bucketBy(100, "user_id").sortBy("user_id").saveAsTable("bucketed_sales")
```


So, instead of storing a giant unsorted table, Spark now stores:

- 100 files (or buckets)
    
- Each file contains only a subset of the `user_id`s (based on a hash)
    
- And each file is **internally sorted** by `user_id`


What is the point ? 

- It knows that all rows with `user_id = 42` will be in bucket #42 of both tables.
- So it can just **merge bucket file #42 from both tables** without shuffling.

When you then read bucketed_sales and join it with another table that is also bucketed on user_id with the same number of buckets, Spark can **completely skip the shuffle phase** of the join.

**When to use it:**

- On tables that are frequently joined together on the same key. It's an investment made at write time to speed up read times.
    

**Pros:**

- Extremely performant for subsequent joins, as it eliminates the shuffle.
    

**Cons / Trade-offs:**

- Rigid: both tables must be bucketed on the same key with the same number of buckets.
    
- Slightly slower write times.
    
- Like repartitioning, it does not solve the underlying key skew problem. A hot key will still create a hot bucket. It's more of a shuffle-avoidance technique.


#### 6. Range Partitioning or Custom Partitioners (The Expert's Tool)

**What it is:** The ultimate control, allowing you to define exactly how keys are mapped to partitions. This is primarily an RDD-level concept.

**How it works:** You create a custom Partitioner class that inherits from spark.Partitioner and implements the getPartition method. You then use this with rdd.partitionBy(customPartitioner).
This is only possible at the **RDD level**, because Spark DataFrames and Datasets rely on the Catalyst optimizer, which abstracts away partitioning logic

**When to use it:**

- Extremely rare in modern DataFrame-based Spark development.

- Useful when your keys have a known, ordered distribution (e.g., numbers from 1 to 1,000,000) and you want to create partitions based on specific ranges (0-100k, 100k-200k, etc.) to ensure perfect balance.

**Pros:**

- Total control over data distribution.
    

**Cons / Trade-offs:**

- Very complex to implement.
    
- Requires dropping down to the RDD API, losing the benefits of the Catalyst Optimizer.
    
- Brittle: if the data distribution changes, your custom partitioner might become ineffective or even create skew.


**Interview Standpoint :** 

Range partitioning and custom partitioners are last-resort tools — used only when you know the data distribution very well and other Spark features like bucketing or AQE can’t solve your skew problems. They’re powerful but dangerous, and usually belong in RDD-level systems for niche cases like massive range scans or IoT time-series data.

### Summary and Decision Framework

|   |   |   |   |   |
|---|---|---|---|---|
|Technique|Best Use Case|Complexity|Effectiveness|Key Takeaway|
|**Broadcast Join**|Large-to-Small joins|Low|Very High|The first thing to check. Avoids the shuffle completely.|
|**Salting**|Large-to-Large joins with severe key skew|High|Very High|The classic manual fix. AQE might do it automatically.|
|**Pre-Filtering Skewed Keys**|Large-to-Large joins with few known hot keys|Medium|High|A targeted "divide and conquer" strategy.|
|**Repartitioning**|Fixing input skew / Controlling output files|Low|Low (for skew)|A blunt instrument, not a targeted solution for key-based skew.|
|**Bucketing**|Tables frequently joined on the same key|Medium|High (for reads)|A physical layout optimization to avoid future shuffles. Not a direct skew fix.|
|**Custom Partitioner**|Keys with a known, ordered distribution|Very High|Very High|The expert's tool. Rarely needed and requires using the RDD API.|

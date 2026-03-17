---
{"dg-publish":true,"permalink":"/personal-vault/data-engineering/structured-streaming/"}
---



Sample WordCount in structured streaming.

```python

#word count

from pyspark.sql import SparkSession
from pyspark.sql.functions import explode, split, col

def main():
    sparksession = SparkSession.builder \
    .appName("Streaming") \
    .getOrCreate()

    sparksession.sparkContext.setLoglevel("ERRORS")

    #lines is a dataframe that has input stream
    lines = sparksession.readStream \
    .format("socket") \
    .option("host", "localhost") \
    .option("port" , "8080") \
    .load()

    #splitting lines into words
    words = lines.select(
        explode(
            #giving the value that it needs to seperate by space.
            split(lines.value, " ")
        ).alias("word")
    )

    # Generate running word count
    wordCounts = words.groupBy("word").count()

    #output modes -> append , complete  and update
    query = wordCounts \
        .writeStream \
        .format("console") \
        .start()
    
    query.awaitTermination()

    if __name__== "__main__":
        main()
```

### How would you design a real-time analytics system for monitoring user activities on an e-commerce platform ? 

**Answer**: I would use a combination of Apache Kafka for data ingestion, Apache Flink or Spark Structured Streaming for real-time data processing, and a database or data warehouse (like Cassandra or Redshift) for storing the results. The system would include components for data cleaning, enrichment, aggregation, and real-time alerting.

```python

spark = sparksession.builder.appName("Monitoring").getOrCreate()


# Read from the Kafka topic 'user_activity'
df = spark.readStream \
    .format("kafka") \
    .option("kafka.bootstrap.servers", "localhost:9092") \
    .option("subscribe", "user_activity") \
    .load()
		
# Define the schema for the user activity data
	schema = StructType([
	    StructField("user_id", StringType(), True),
	    StructField("event_type", StringType(), True),
	    StructField("timestamp", TimestampType(), True),
	    StructField("page_url", StringType(), True),
	    StructField("product_id", StringType(), True),
	    StructField("session_id", StringType(), True)
	])
	
# Parse the JSON data and extract the fields
	user_activities = df.selectExpr("CAST(value AS STRING) as json") \
	    .select(from_json(col("json"), schema).alias("data")) \
	    .select("data.*")

#initiate a query and write into kafka topic , also checkpoint.

	query = user_activities.writeStream \ 
	.format("kafka") \ 
	.option("kafka.bootstrap.servers", "localhost:9092") \ 
	.option("topic", "output_topic") \ 
	.option("checkpointLocation", "/path/to/checkpoint/dir") \ 
	.start()

#await termination
query.awaitTermination()
```

How would you implement exactly-once semantics in a streaming application? 

I woud use combination of idempotent writes,  checkpointing , and transactional writes.

#### Scenario-Based Questions:

- You notice that your streaming application has high latency. What steps would you take to diagnose and reduce the latency ?
	- Identify the bottlenecks ( checking the input data rate , ensuring proper partitioning , optimizing transformations , tuning cluster resources , check pointing intervals)
- Describe a situation where you had to handle out-of-order data in a stream processing application. How did you handle it?
	-  I will use watermarking , and will set a threshold for the out of order  arrival data. ( event time windows in flink )
- How does flink achieve fault tolerance 
	- using check pointing , snapshots the state of the application and saves it in a consistant storage
- **Explain the differences between Flink's and Spark's approach to stream processing.**
	-  Flink natively supports stream processing with a low-latency, event-driven architecture, and robust state management. It offers sophisticated features like event time processing and custom windowing. Spark Structured Streaming, on the other hand, treats streaming as a series of micro-batches and leverages the Spark SQL engine for its operations, making it easier for users familiar with batch processing but potentially higher in latency compared to Flink.
- 
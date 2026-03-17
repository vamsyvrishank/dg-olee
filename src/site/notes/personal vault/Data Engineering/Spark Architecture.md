---
{"dg-publish":true,"permalink":"/personal-vault/data-engineering/spark-architecture/"}
---



![[Pasted image 20240610141031.png \| 800]]

## Spark Driver

Spark driver is a program that runs on the master node of the machine which declares transformations and actions on knowledge RDDs. In easy terms, the driver in Spark **creates SparkContext, connected to a given Spark Master**

SparkSession is the recommended way to work with Spark data structures such as DataFrames and Datasets, as it provides a more consistent and simpler interface. SparkContext is still useful for working with RDDs and low-level Spark features, but it can also be accessed through SparkSession if needed

## SparkSession

SparkSession is the recommended way to work with Spark data structures such as DataFrames and Datasets, as it provides a more consistent and simpler interface. SparkContext is still useful for working with RDDs and low-level Spark features, but it can also be accessed through SparkSession if needed

Now with sparkSession you can access multiple contexts such as SparkContext, SQLContext, HiveContext, SparkConf, and StreamingContext.

![[Pasted image 20240610141543.png \| 800]]

## Cluster Manager

The cluster manager is responsible for managing and allocating resources for the cluster of nodes on which your Spark application runs. Currently, Spark supports four cluster managers: the built-in standalone cluster manager, Apache Hadoop YARN, Apache Mesos, and Kubernetes


## Spark Executor

A Spark executor runs on each worker node in the cluster. The executors communicate with the driver program and are responsible for executing tasks on the workers. In most deployments modes, only a single executor runs per node.

## Deployment Modes

An attractive feature of Spark is its support for myriad deployment modes, enabling Spark to run in different configurations and environments.

![[Pasted image 20240610141852.png \| 800]]

## Distributed data and partitions

Actual physical data is distributed across storage as partitions residing in either HDFS or cloud storage . While the data is distributed as partitions across the physical cluster, Spark treats each partition as a high-level logical data abstraction—as a DataFrame in memory. Though this is not always possible, each Spark executor is preferably allocated a task that requires it to read the partition closest to it in the net‐ work, observing data locality.

Partitioning allows for efficient parallelism. A distributed scheme of breaking up data into chunks or partitions allows Spark executors to process only data that is close to them, minimizing network bandwidth. That is, each executor’s core is assigned its own data partition to work on.

![[Pasted image 20240610142059.png \| 800]]




For example, this code snippet will break up the physical data stored across clusters into eight partitions, and each executor will get one or more partitions to read into its memory:

![[Pasted image 20240610142208.png \| 800]]

```python
log_df = spark.read.text("path_to_large_text_file").repartition(8) 
print(log_df.rdd.getNumPartitions())
```

And this code will create a DataFrame of 10,000 integers distributed over eight partitions in memory:
```python
df = spark.range(0, 10000, 1, 8) 
print(df.rdd.getNumPartitions())
```

Both of these will print 8.

Repartition will break the data into partition across clusters.
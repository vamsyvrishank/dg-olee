---
{"dg-publish":true,"permalink":"/personal-vault/data-engineering/overview/"}
---


<mark class="hltr-red">Limitations of Hadoop </mark>

Even though Hadoop MR was conducive to large-scale jobs for general batch processing, it fell short for combining other workloads such as machine learn‐ ing, streaming, or interactive SQL-like queries.
- no in memory storage 
- not for small sized data
- no caching / not for streaming as well
- 



<mark class="hltr-red">Spark</mark>

- Spark is a unified engine designed for large-scale distributed data processing, on premises in data centers or in the cloud.
- Spark provides in-memory storage for intermediate computations, making it much faster than Hadoop MapReduce.
- It incorporates libraries with composable APIs for machine learning (MLlib), SQL for interactive queries (Spark SQL), stream processing (Structured Streaming) for interacting with real-time data, and graph processing (GraphX).
- Spark builds its query computations as a directed acyclic graph (DAG); its DAG scheduler and query optimizer construct an efficient computational graph that can usually be decomposed into tasks that are executed in parallel across workers on the cluster. 
- And third, its physical execution engine, Tungsten, uses whole-stage code generation to generate compact code for execution
- Spark achieves simplicity by providing a fundamental abstraction of a simple logical data structure called a <mark class="hltr-green">Resilient Distributed Dataset (RDD)</mark> upon which all other higher-level structured data abstractions, such as DataFrames and Datasets, are constructed.
- Spark focuses on its fast, parallel computation engine rather than on storage. Unlike Apache Hadoop, which included both storage and compute, Spark decouples the two.


> [!NOTE] RDD
> An RDD is an immutable distributed collection of datasets partitioned across a set of nodes of the cluster that can be recovered if a partition is lost, thus providing fault tolerance


![Pasted image 20240610140630.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240610140630.png)


Originally, **Spark was designed for batch processing and Kafka was designed for stream processing**. Later on, Spark added the Spark Streaming module as an add-on to its underlying distributed architecture. However, Kafka offers<mark class="hltr-green"> lower latency and higher throughput for most streaming data use cases</mark>.


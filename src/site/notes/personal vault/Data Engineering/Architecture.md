---
{"dg-publish":true,"permalink":"/personal-vault/data-engineering/architecture/"}
---


source -> https://www.geeksforgeeks.org/hadoop-architecture/

Hadoop has 4 components:

- MapReduce
- HDFS(Hadoop Distributed File System)
- YARN(Yet Another Resource Negotiator)
- Common Utilities or Hadoop Common

![[Pasted image 20240608211726.png \| 600]]


## MapReduce( ) : 


Main task is to perform distributed processing in parallel in Hadoop Cluster.

![[Pasted image 20240608211916.png \| 400]]

Map Task :

- RecordReader : The purpose of _recordreader_ is to break the records. It is responsible for providing key-value pairs in a Map() function
- Map : A map is nothing but a user-defined function whose work is to process the Tuples obtained from record reader.
- Combiner : Combiner is used for grouping the data in the Map workflow ,  similar to local reducer , generates intermediate keys
- Partitioner : Partitioner is responsible for fetching key-value pairs generated in the Mapper Phases. The partitioner generates the shards corresponding to each reducer. Hashcode of each key is also fetched by this partition. Then partitioner performs it’s(Hashcode) modulus with the number of reducers(_key.hashcode()%(number of reducers))._


Reduce Task:

- Shuffle and Sort :  The Task of Reducer starts with this step, the process in which the Mapper generates the intermediate key-value and transfers them to the Reducer task is known as _Shuffling_. Using the Shuffling process the system can sort the data using its key value. Once some of the Mapping tasks are done Shuffling begins that is why it is a faster process and does not wait for the completion of the task performed by Mapper.
- Reduce : The main function or task of the Reduce is to gather the Tuple generated from Map and then perform some sorting and aggregation sort of process on those key-value depending on its key element.
- Output Format : Once all the operations are performed, the key-value pairs are written into the file with the help of record writer, each record in a new line, and the key and value in a space-separated manner.



![[Pasted image 20240608212836.png \| 600]]


## HDFS

HDFS(Hadoop Distributed File System) is utilized for storage permission. It is mainly designed for working on commodity Hardware devices(inexpensive devices), working on a distributed file system design. HDFS is designed in such a way that it believes more in storing the data in a large chunk of blocks rather than storing small data blocks

HDFS in Hadoop provides Fault-tolerance and High availability to the storage layer and the other devices present in that Hadoop cluster. Data storage Nodes in HDFS.

- NameNode(Master)
- DataNode(Slave)

NameNode : NameNode works as a Master in a Hadoop cluster that guides the Datanode(Slaves). Namenode is mainly used for storing the Metadata i. Meta Data can be the transaction logs that keep track of the user’s activity in a Hadoop cluster, name of the file, size, and the information about the location(Block number, Block ids) of Datanode that Namenode stores to find the closest DataNode for Faster Communication. Namenode instructs the DataNodes with the operation like delete, create, Replicate, etc.

DataNode : DataNodes works as a Slave DataNodes are mainly utilized for storing the data in a Hadoop cluster, the number of DataNodes can be from 1 to 500 or even more than that. The more number of DataNode, the Hadoop cluster will be able to store more data. So it is advised that the DataNode should have High storing capacity to store a large number of file blocks.

<mark class="hltr-pink">High Level Architecture Of Hadoop</mark>

![[Pasted image 20240608213347.png \| 900]]


## YARN

- YARN is a Framework on which MapReduce works. YARN performs 2 operations that are Job scheduling and Resource Management.
- The Purpose of Job schedular is to divide a big task into small jobs so that each job can be assigned to various slaves in a Hadoop cluster and Processing can be Maximized
- Job Scheduler also keeps track of which job is important, which job has more priority, dependencies between the jobs and all the other information like job timing, etc

**Features of YARN** 

- Multi-Tenancy
- Scalability
- Cluster-Utilization
- Compatibility


## Hadoop Common

- Hadoop common or Common utilities are nothing but our java library and java files or we can say the java scripts that we need for all the other components present in a Hadoop cluster
-  These utilities are used by HDFS, YARN, and MapReduce for running the cluster
- 
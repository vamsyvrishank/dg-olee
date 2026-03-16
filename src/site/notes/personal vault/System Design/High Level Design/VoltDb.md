---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/volt-db/"}
---



### NewSQL 

| **Traditional Relational DB** | **Newer Types Relational DB** |
| ----------------------------- | ----------------------------- |
| MySQL                         | VoltDB                        |
| PostgreSQL                    | Spanner                       |

Extra Twist added in this. Everything comes down to this " How do we speed up the transactions ? "


| MySQL                                                                                                                    | Postgres                                                                                                                                                                                                                                              | VoltD                                                                                                                                                                                                                                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2PL                                                                                                                      | Serializable Snapshot Isolation                                                                                                                                                                                                                       | Actual Serial Execution<br>**Execution Model:**<br><br>**Single CPU Thread**: VoltDB executes transactions serially in a single CPU thread, which eliminates the need for locks and ensures true serializability. This model is very fast but can become a bottleneck because all transactions are limited by the speed of a single CPU core. |
| Every row has locks either shared(reads)<br> or exclusive(writes)<br><br>![[Pasted image 20240811155422.png \| 150]]<br> | We read from consistent snapshot<br>and db keeps track of other transactions reading the same data , <br>if any transaction commits , then it is rolled back and retried later <br>-Obviously slow<br><br>![[Pasted image 20240811155445.png \| 350]] | ![[Pasted image 20240811154828.png \|  150]]                                                                                                                                                                                                                                                                                                  |
|                                                                                                                          |                                                                                                                                                                                                                                                       | queue of things to do in a one single CPU thread<br>Very fast but bottleneck                                                                                                                                                                                                                                                                  |
|                                                                                                                          |                                                                                                                                                                                                                                                       | Problem : Bottleneck is CPU.<br><br>**Bottlenecks Beyond CPU:**<br><br>1. **Hard Drives**: Traditionally slow and unreliable, disks can severely limit database performance.<br>2. **Network Latency**: Delays in data transmission between distributed nodes can also hinder performance                                                     |


### Dealing with Disks: In-Memory Storage

- **In-Memory Data**: VoltDB stores all data in RAM, avoiding the latency issues associated with disk I/O. This allows for extremely fast read and write operations using hash indexes with O(1) time complexity.

**Pros:**

- **Fast Indexing**: Hash indexes in memory enable O(1) complexity for reads and writes.
- **Optional Write-Ahead Log (WAL)**: Provides durability without compromising performance, with support for range queries via optional tree or sorted set indexes with O(log(n)) complexity.

**Cons:**

- **Limited Data Per Node**: RAM is expensive and limited in capacity, meaning that as data grows, more partitions are needed to store it.
- **Challenges with Partitions**(2PC):
    - **Cross-Partition Operations**: In a distributed SQL database like VoltDB, more partitions lead to more cross-partition reads and writes, which are slower and more complex to manage.
    - **Distributed Complexity**: Coordinating transactions across multiple partitions introduces latency and potential for contention, complicating the system and reducing the benefits of fast, single-threaded execution.


### Dealing with Network Latency

### **Batching and Pipelining**

- **Batch Processing**: Group multiple operations into a single network call to reduce the overhead of multiple round trips. This can significantly lower the total time spent waiting for data transfer.
- **Pipelining Requests**: Send multiple requests in parallel without waiting for the previous one to complete. This overlaps communication and computation, reducing the impact of latency.


### Conclusion

It is like a case study than a real life example. we probably wouldn't need this in interview.
VoltDb is a very interesting approach to achieving ACID transactions without 2PL or SSI - However it must make a lot of sacrifices because of this.
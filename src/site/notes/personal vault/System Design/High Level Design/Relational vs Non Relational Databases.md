---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/relational-vs-non-relational-databases/"}
---


#### Relational Databases (RDBMS)

Relational databases use **normalized data**, organizing it into structured tables to reduce redundancy and ensure integrity. However, this often leads to **poor data locality**, where related data is spread across multiple tables, sometimes on different nodes. This can cause:

- **Distributed Transactions**: Operations that span multiple nodes require distributed transactions, often using slow two-phase commits.
- **Slow Reads (Joins)**: Complex joins across tables can be slow due to the need to access data from multiple systems.

#### Non-Relational Databases (NoSQL)

Non-relational databases, often associated with **NoSQL**, use **denormalized data** to store related information together, improving data locality. This leads to:

- **Better Data Locality**: Faster read and write operations since related data is stored together, reducing the need for distributed transactions.
- **Data Redundancy**: While improving performance, denormalisation results in data duplication, which can increase storage and consistency challenges.

#### Misconceptions:

The terms **SQL** and **NoSQL** are not synonymous with relational and non-relational databases. Some NoSQL databases support SQL-like queries, and relational databases may include NoSQL features.

### Non-Relational Database Trade-offs

Non-relational (NoSQL) databases are designed to handle large-scale, distributed data sets, often in scenarios where traditional relational databases may struggle. However, they come with specific trade-offs:

- **Avoiding Cross-Partition Reads**: Non-relational databases typically store denormalized data, meaning related information is kept together within the same document or partition. This design minimizes the need for cross-partition reads, which can be expensive in terms of latency and performance, especially in distributed systems. By keeping data localized, these databases reduce the overhead of querying multiple partitions or nodes, leading to faster data retrieval.
    
- **Increased Write Complexity**: The use of denormalized data, while beneficial for read operations, increases the complexity and frequency of write operations. Every update to a piece of data might require updates to multiple documents or records. In a distributed environment, this can necessitate the use of distributed transactions, which are generally more complex and slower compared to local transactions. Technologies like two-phase commit (2PC) or consensus algorithms like Raft may be required to ensure consistency, adding further overhead.
    
- **Network Overhead**: In non-relational databases, data is often stored in large, self-contained documents (e.g., JSON in MongoDB). When these documents need to be transmitted across the network, the entire document is often sent, even if only a small portion is needed. This can lead to significant network overhead, particularly in applications with large or frequently accessed documents. Compression and differential synchronization strategies are sometimes used to mitigate this, but these add additional complexity.

#### Types of NoSQL Databases:
- **Document Stores:** 
  - Like MongoDB, where data is stored in documents (JSON-like structures). Each document contains all the related data in one place, which is great for hierarchical or nested data.
- **Key-Value Stores:** 
  - Like Redis, where each piece of data is stored as a key-value pair. This is simple and fast for lookup operations but can be limited in functionality.
- **Column Stores:** 
  - Like Cassandra, which stores data in columns rather than rows, making it efficient for read-heavy operations that require specific columns from a large dataset.
- **Graph Databases:** 
  - Like Neo4j, designed to manage and query data that is interconnected, making it ideal for scenarios like social networks or recommendation engines.

### Conclusion

Non-relational databases are ideal for scenarios involving highly decoupled, large-scale, and non-relational data, offering advantages in terms of performance and scalability. However, they also introduce challenges such as increased write complexity and network overhead.

It's essential to understand that not all databases within the relational or non-relational category are the same. They vary widely in terms of data structures, indexing strategies, and replication mechanisms. Therefore, it's crucial to evaluate specific databases based on their individual characteristics and how well they align with the application's requirements. Broad assumptions about the nature of relational versus non-relational databases can be misleading and overlook the nuances that define their real-world performance and applicability.







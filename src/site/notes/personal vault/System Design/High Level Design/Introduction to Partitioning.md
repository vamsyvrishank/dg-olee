---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/introduction-to-partitioning/"}
---



![[Pasted image 20240803144532.png \| 500]]

break db into pieces.

### Range Based Partitioning

![[Pasted image 20240803144915.png \| 500]]

pro : range queries are good.
cons : hot spots 

people will common names will fill up the specific partition.


### Hash Range Based Partitioning

![[Pasted image 20240803145113.png \| 500]]

Pros : relatively even distributions
cons : no data locality for range queries

lets say on ig : hash ( post -> post id ) and comments were stored with each post id.
we will have ton of comments on 1 post and that would create a hot spot if certain keys are very active.

#### We can use a secondary Index 

In the context of partitioned databases, such as Amazon DynamoDB, Local Secondary Indexes (LSIs) and Global Secondary Indexes (GSIs) are powerful tools that allow for efficient querying and data access patterns beyond the primary key schema of a table. Here’s a detailed explanation of both:

### Local Secondary Index (LSI)

**Definition**: An LSI is an index that has the same partition key as the table but allows for a different sort key. This means that LSIs are scoped to the same partition and can provide alternative views on the data within each partition.

**Characteristics**:

1. **Same Partition Key**: LSIs share the same partition key as the base table.
2. **Different Sort Key**: They can have a different sort key, which allows for querying data based on a different attribute while still grouping by the same partition key.
3. **Created at Table Creation**: LSIs must be defined at the time of table creation and cannot be added or removed later.
4. **Size Limitations**: There is a 10 GB limit per partition for the total data size in an LSI, including the base table and all LSIs.

**Use Case**: LSIs are useful when you need to query data within the same partition but need to sort or filter it differently. For example, if you have a `Users` table with a partition key of `UserId` and a sort key of `CreationDate`, you could create an LSI with a sort key of `LastLoginDate` to efficiently query users based on their last login time.

### Global Secondary Index (GSI)

**Definition**: A GSI is an index that allows for both a different partition key and sort key from the base table. This provides much more flexibility as it enables querying across all partitions.

**Characteristics**:

1. **Different Partition and Sort Keys**: GSIs can have completely different partition and sort keys from the base table, which allows for more diverse query patterns.
2. **Created Anytime**: GSIs can be created when the table is created or added to an existing table at any time.
3. **No Size Limitations**: Unlike LSIs, GSIs do not have a 10 GB limit per partition

**Use Case**: GSIs are ideal for queries that require different partition keys. For example, in an `Orders` table with a partition key of `OrderId` and a sort key of `OrderDate`, you could create a GSI with a partition key of `CustomerId` and a sort key of `OrderDate` to query all orders made by a specific customer.
---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/my-sql-vs-postgre-sql/"}
---





### SQL Databases 

We are generally using relational db because we have relational data , but can we use relational db even if we do not have relational data , data we need to store in a denormalized fashion ? 
Yes , only sometimes. 

**Common features** 
	1. B Tree Based Index - better for reads.
		1. ![[Pasted image 20240811121548.png \| 500]]
	2. Single Leader replication - No write conflicts 
	3. Configurable Isolation Levels  - Isolation levels are part of the **ACID** properties (Atomicity, Consistency, Isolation, Durability) that ensure the reliability of transactions in a database. The isolation level dictates how strictly transactions are isolated from each other, which can affect performance and concurrency.

#### Common Isolation Levels

>[!Note]
>- **Read Uncommitted**:
  >  
  > 	 - Transactions can read data that is not yet committed by other transactions. This can lead to "dirty reads," where a transaction sees uncommitted changes that might be rolled back.
   > 	- **Use Case**: High concurrency is needed, and data accuracy is less critical.
>- **Read Committed**:
    >
  > 	 - A transaction can only read data that has been committed by other transactions. This prevents dirty reads but allows for "non-repeatable reads," where the same query in the same transaction can return different results if another transaction modifies the data and commits between the queries.
    >- **Use Case**: Balances data consistency with performance.
>- **Repeatable Read**: 
 > 	 - A transaction ensures that if it reads a row of data, subsequent reads within the same transaction will return the same data, even if another transaction commits changes to that data in the meantime. However, this does not prevent "phantom reads," where new rows added by another transaction can appear in subsequent queries.
   > 	- **Use Case**: When repeatable reads are essential, but full serialization isn't necessary.
>- **Serializable**:
 > 	  - The strictest isolation level, where transactions are executed in such a way that they appear to be running serially, one after the other. This level prevents dirty reads, non-repeatable reads, and phantom reads, ensuring the highest level of consistency but at the cost of concurrency and performance.
   > 	- **Use Case**: When absolute data consistency is critical, and performance can be sacrificed.


### Isolation Differences

| **MySQL**                                         | **PostGreSQL**                                                                                                                       |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Two Phase Locking                                 | Serializable Snapshot Isolation                                                                                                      |
| Every row has locks                               | Transactions are read from data snapshots                                                                                            |
| Read only transactions grab locks  in shared mode | If transactions reads value which is modified by another transaction before committing , <br>original values needs to be rolled back |
| To write must grab exclusive mode                 |                                                                                                                                      |
| Lots of deadlocks to detect and undo              |                                                                                                                                      |




### Conclusions

- Use SQL db for data when you need data to be normalised and for data that has to be correct. if you need ACID compliance transactions , avoid race conditions , avoid write conflicts , use SQL databases.
- In theory , SSI > 2PL , (postgres > Mysql) , however if there are a lot of conflicting transactions , pessimistic locking may be better ! 


### MySQL vs. PostgreSQL: A Comprehensive Comparison

When choosing a relational database management system (RDBMS), MySQL and PostgreSQL are two of the most popular options. Each has its strengths and weaknesses, making them suitable for different use cases depending on the specific requirements of your project. Here’s a detailed comparison to help you decide which might be the best fit.

### MySQL: Overview, Pros, and Cons

**Overview**:
MySQL is a widely-used, open-source relational database that’s known for its speed, reliability, and ease of use. It’s particularly popular for web applications, including those built on the LAMP stack (Linux, Apache, MySQL, PHP/Python/Perl).

**Pros**

- **Speed and Simplicity**: MySQL is optimized for read-heavy operations and is often faster for simple queries, making it a great choice for web applications.
- **Widely Supported**: MySQL has extensive support in various frameworks, tools, and hosting environments. It integrates seamlessly with PHP, making it a go-to choice for many web developers.
- **Replication**: MySQL offers robust master-slave replication, allowing easy scalability for read-heavy applications.
- **Community and Resources**: As one of the most popular databases, MySQL has a vast community and extensive documentation, making it easier to find support and resources.

**Cons**:

- **Limited Advanced Features**: MySQL lacks some of the advanced features found in PostgreSQL, such as full-text search, custom types, and advanced indexing options.
- **ACID Compliance**: While MySQL supports ACID transactions, it’s not as robust as PostgreSQL, particularly in its default storage engine (InnoDB). In some cases, this can lead to data integrity issues under high concurrency.
- **Concurrency**: MySQL’s performance can degrade under heavy write operations, particularly in high-concurrency environments.

**When to Use MySQL**:

- **Web Applications**: MySQL is ideal for web applications that require fast read operations, such as content management systems (WordPress, Drupal) and e-commerce platforms (Magento, WooCommerce).
- **Small to Medium-Sized Projects**: For smaller projects where simplicity and speed are more critical than advanced features, MySQL is often the better choice.
- **Read-Heavy Workloads**: Applications with more reads than writes can benefit from MySQL’s speed and replication features.

**When Not to Use MySQL**:

- **Complex Transactions**: If your application requires complex transactions, such as those involving multiple tables and a high degree of concurrency, PostgreSQL might be a better fit.
- **Data Integrity**: Applications that require strict adherence to ACID properties, especially under high loads, may find PostgreSQL’s transaction management more reliable.

### PostgreSQL: Overview, Pros, and Cons

**Overview**:
PostgreSQL is an advanced, open-source relational database known for its emphasis on extensibility and standards compliance. It’s often the preferred choice for complex applications that require advanced features and robust data integrity.

**Pros**:

- **Advanced Features**: PostgreSQL supports a wide range of advanced features, including custom data types, full-text search, JSONB support, geospatial data (PostGIS), and more. This makes it suitable for complex applications.
- **ACID Compliance**: PostgreSQL is fully ACID-compliant, ensuring robust transaction handling even under heavy loads and high concurrency.
- **Extensibility**: PostgreSQL allows for extensive customization, including the creation of custom functions, types, and operators. This makes it highly adaptable to specialized use cases.
- **Data Integrity and Concurrency**: PostgreSQL handles high-concurrency environments better than MySQL, making it suitable for applications with heavy write operations.

**Cons**:

- **Performance**: While PostgreSQL is powerful, it can be slower than MySQL for simple read-heavy operations, especially in less complex environments.
- **Complexity**: PostgreSQL’s advanced features come with added complexity, which might be overkill for simpler applications. It can also require more tuning and maintenance.
- **Resource Usage**: PostgreSQL tends to consume more resources (CPU, memory) than MySQL, which could be a concern in resource-constrained environments.

**When to Use PostgreSQL**:

- **Complex Applications**: Applications that require complex queries, custom data types, or advanced features like full-text search or geospatial data should consider PostgreSQL.
- **High-Concurrency Environments**: For systems with heavy write loads and a need for robust transaction handling, PostgreSQL’s concurrency management shines.
- **Data Warehousing and Analytics**: PostgreSQL’s ability to handle complex queries and large datasets makes it a strong candidate for data warehousing and analytics applications.

**When Not to Use PostgreSQL**:

- **Simple, Read-Heavy Workloads**: If your application is simple and primarily involves read operations, MySQL might be faster and easier to manage.
- **Limited Resources**: In environments where resources are limited, and the advanced features of PostgreSQL aren’t needed, MySQL’s lighter resource footprint might be advantageous.

### Cost Considerations

Both MySQL and PostgreSQL are open-source and free to use, but there are cost considerations when it comes to hosting, maintenance, and support:

- **MySQL**:
  - **Cloud Hosting**: MySQL is widely available on cloud platforms like AWS (RDS), Google Cloud, and Azure. Pricing for managed MySQL services is generally competitive.
  - **Enterprise Edition**: Oracle offers a commercial version of MySQL with additional features and support, but this comes at a cost.
  
- **PostgreSQL**:
  - **Cloud Hosting**: PostgreSQL is also widely supported by cloud platforms, and like MySQL, managed services are available on AWS (RDS), Google Cloud, and Azure.
  - **Enterprise Support**: There are several companies that offer commercial support for PostgreSQL (e.g., EnterpriseDB), which can add to the cost but provide peace of mind for mission-critical applications.

### Real-World Use Cases

**MySQL**:
- **WordPress**: The most popular content management system globally relies on MySQL for its database needs.
- **YouTube**: Originally, YouTube used MySQL as its primary database, which demonstrates its ability to handle large-scale web applications.
- **Facebook**: While Facebook primarily uses MySQL, it has made significant customizations to meet its massive scale and specific needs.

**PostgreSQL**:
- **Instagram**: Instagram relies on PostgreSQL for its ability to handle complex queries and massive amounts of data.
- **Reddit**: The social news aggregation site uses PostgreSQL for its advanced features and reliability.
- **Apple**: Apple uses PostgreSQL in various internal projects due to its extensibility and standards compliance.


### PostgreSQL vs. MySQL

| **Criteria**             | **PostgreSQL**                                                                                           | **MySQL**                                                                                                    |
| ------------------------ | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **Strengths**            | - Advanced SQL compliance with full ACID support                                                         | - High performance in read-heavy workloads                                                                   |
|                          | - Extensible with support for custom data types and functions                                            | - Simpler to set up and use                                                                                  |
|                          | - Superior support for complex queries and large datasets                                                | - Wide adoption and extensive community support                                                              |
|                          | - Rich indexing options and advanced optimizations                                                       | - Optimized for high concurrency and fast read operations                                                    |
|                          | - Advanced data integrity features                                                                       | - Built-in replication features                                                                              |
|                          | - Superior transaction handling with SSI                                                                 | - Multiple storage engines (e.g., InnoDB, MyISAM)                                                            |
| **Weaknesses**           | - Can be slower in write-heavy workloads with high contention due to SSI rollbacks                       | - Less strict SQL compliance and fewer advanced features compared to PostgreSQL                              |
|                          | - More complex to configure and maintain                                                                 | - Basic transactional integrity in some storage engines                                                      |
|                          | - Higher memory usage in some scenarios                                                                  | - Lacks some of PostgreSQL's advanced indexing and optimization features                                     |
| **Ideal Use Cases**      | - Applications requiring complex queries, data integrity, and compliance (e.g., financial systems)       | - High-traffic websites with read-heavy workloads (e.g., content management systems)                         |
|                          | - Systems needing custom data types, functions, and extensions (e.g., GIS applications with PostGIS)     | - Applications where speed and simplicity are priorities over advanced features (e.g., e-commerce platforms) |
|                          | - Large-scale data warehousing and analytics                                                             | - Scenarios requiring high availability and replication (e.g., distributed systems)                          |
| **When Not to Use**      | - For simple read-heavy applications where MySQL can be more performant                                  | - When advanced SQL features, complex queries, or data integrity are critical                                |
|                          | - Write-heavy applications with extreme contention might perform better with simpler locking (e.g., 2PL) | - When you need extensive support for complex data types and queries                                         |
|                          | - Scenarios where ease of use and quick setup are prioritized over advanced features                     | - Write-intensive workloads where deadlocks and lock contention could become an issue                        |
| **Cost**                 | - Open-source and free to use; can have higher operational costs due to complexity                       | - Open-source and free to use; generally lower operational costs due to simplicity                           |
|                          | - Commercial support available (e.g., from EnterpriseDB)                                                 | - Commercial support available (e.g., from Oracle)                                                           |
| **Real-World Use Cases** | - Financial systems (e.g., banking software, payment processing)                                         | - Content management systems (e.g., WordPress)                                                               |
|                          | - Geographic Information Systems (GIS) with PostGIS                                                      | - E-commerce platforms (e.g., Magento)                                                                       |
|                          | - Large-scale analytics platforms                                                                        | - Social networking sites (e.g., Facebook initially used MySQL)                                              |
|                          | - Government and healthcare databases                                                                    | - High-traffic websites and applications needing quick reads                                                 |
| **Mechanisms Used**      | - **SSI (Serializable Snapshot Isolation)**: Ensures high isolation and consistency, avoiding deadlocks  | - **2PL (Two-Phase Locking)**: Provides strict locking for write operations, leading to potential deadlocks  |
|                          | - **MVCC (Multi-Version Concurrency Control)**: Allows non-blocking reads, improving read efficiency     | - **InnoDB's MVCC**: Similar to PostgreSQL but with different implementation, focusing on quick reads        |
|                          | - **Advanced Indexing (e.g., B-trees, GIN, BRIN)**: Enhances performance for complex queries             | - **Read-Optimized Architecture**: Simple and efficient for high concurrency read operations                 |
|                          | - **Custom Data Types and Extensions**: Supports a wide range of use cases with tailored solutions       | - **Replication Mechanisms**: Built-in support for master-slave replication for high availability            |

### Summary:

- **PostgreSQL** is ideal for applications that require advanced SQL features, complex queries, and strong data integrity, especially in write-heavy or analytically intensive environments. Its strength lies in its adherence to SQL standards, extensibility, and superior transaction handling. However, it may require more complex configuration and might not be as performant as MySQL in simple, read-heavy workloads.

- **MySQL** excels in environments where simplicity, speed, and high read performance are crucial, such as in web applications and content management systems. It’s easier to set up and use, making it a popular choice for smaller or less complex applications. However, it might not be suitable for use cases that require advanced SQL features or high transaction integrity under heavy write contention. 



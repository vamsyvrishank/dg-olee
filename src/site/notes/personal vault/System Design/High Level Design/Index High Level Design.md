---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/index-high-level-design/"}
---



System Design Key Concepts

1. [[personal vault/System Design/High Level Design/Contents/Scalability\|Scalability]]
2. [[Latency vs Throughput \|Latency vs Throughput ]]
3. [[Cap Theorem\|Cap Theorem]]
4. [[ACID Transactions\|ACID Transactions]]
5. [[personal vault/Data Structures and Algorithms/To be Revisited/Consistent Hashing\|Consistent Hashing]]
6. [[personal vault/System Design/High Level Design/Consistency vs Eventual Consistency\|Consistency vs Eventual Consistency]]
7. [[personal vault/System Design/High Level Design/Synchronous vs Asynchronous Communication\|Synchronous vs Asynchronous Communication]]
8. 


Importank Links
1. [[personal vault/System Design/High Level Design/Contents/Redis use cases\|Redis use cases]]








System Design Primer 

- [System design topics: start here](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#system-design-topics-start-here)
    - [Step 1: Review the scalability video lecture](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#step-1-review-the-scalability-video-lecture)
    - [Step 2: Review the scalability article](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#step-2-review-the-scalability-article)
    - [Next steps](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#next-steps)
- [Performance vs scalability](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#performance-vs-scalability)
- [Latency vs throughput](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#latency-vs-throughput)
- [Availability vs consistency](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#availability-vs-consistency)
    - [CAP theorem](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#cap-theorem)
        - [CP - consistency and partition tolerance](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#cp---consistency-and-partition-tolerance)
        - [AP - availability and partition tolerance](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#ap---availability-and-partition-tolerance)
- [Consistency patterns](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#consistency-patterns)
    - [Weak consistency](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#weak-consistency)
    - [Eventual consistency](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#eventual-consistency)
    - [Strong consistency](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#strong-consistency)
- [Availability patterns](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#availability-patterns)
    - [Fail-over](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#fail-over)
    - [Replication](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#replication)
    - [Availability in numbers](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#availability-in-numbers)
- [Domain name system](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#domain-name-system)
- [Content delivery network](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#content-delivery-network)
    - [Push CDNs](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#push-cdns)
    - [Pull CDNs](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#pull-cdns)
- [Load balancer](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#load-balancer)
    - [Active-passive](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#active-passive)
    - [Active-active](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#active-active)
    - [Layer 4 load balancing](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#layer-4-load-balancing)
    - [Layer 7 load balancing](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#layer-7-load-balancing)
    - [Horizontal scaling](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#horizontal-scaling)
- [Reverse proxy (web server)](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#reverse-proxy-web-server)
    - [Load balancer vs reverse proxy](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#load-balancer-vs-reverse-proxy)
- [Application layer](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#application-layer)
    - [Microservices](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#microservices)
    - [Service discovery](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#service-discovery)
- [Database](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#database)
    - [Relational database management system (RDBMS)](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#relational-database-management-system-rdbms)
        - [Master-slave replication](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#master-slave-replication)
        - [Master-master replication](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#master-master-replication)
        - [Federation](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#federation)
        - [Sharding](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#sharding)
        - [Denormalization](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#denormalization)
        - [SQL tuning](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#sql-tuning)
    - [NoSQL](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#nosql)
        - [Key-value store](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#key-value-store)
        - [Document store](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#document-store)
        - [Wide column store](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#wide-column-store)
        - [Graph Database](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#graph-database)
    - [SQL or NoSQL](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#sql-or-nosql)
- [Cache](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#cache)
    - [Client caching](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#client-caching)
    - [CDN caching](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#cdn-caching)
    - [Web server caching](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#web-server-caching)
    - [Database caching](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#database-caching)
    - [Application caching](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#application-caching)
    - [Caching at the database query level](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#caching-at-the-database-query-level)
    - [Caching at the object level](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#caching-at-the-object-level)
    - [When to update the cache](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#when-to-update-the-cache)
        - [Cache-aside](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#cache-aside)
        - [Write-through](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#write-through)
        - [Write-behind (write-back)](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#write-behind-write-back)
        - [Refresh-ahead](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#refresh-ahead)
- [Asynchronism](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#asynchronism)
    - [Message queues](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#message-queues)
    - [Task queues](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#task-queues)
    - [Back pressure](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#back-pressure)
- [Communication](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#communication)
    - [Transmission control protocol (TCP)](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#transmission-control-protocol-tcp)
    - [User datagram protocol (UDP)](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#user-datagram-protocol-udp)
    - [Remote procedure call (RPC)](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#remote-procedure-call-rpc)
    - [Representational state transfer (REST)](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#representational-state-transfer-rest)
- [Security](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#security)
- [Appendix](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#appendix)
    - [Powers of two table](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#powers-of-two-table)
    - [Latency numbers every programmer should know](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#latency-numbers-every-programmer-should-know)
    - [Additional system design interview questions](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#additional-system-design-interview-questions)
    - [Real world architectures](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#real-world-architectures)
    - [Company architectures](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#company-architectures)
    - [Company engineering blogs](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#company-engineering-blogs)
- [Under development](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#under-development)
- [Credits](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#credits)
- [Contact info](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#contact-info)
- [License](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#license)




Theory

1. [[personal vault/System Design/High Level Design/Database Indexes\|Database Indexes]]
    1. [[personal vault/System Design/How hash indexes work\|How hash indexes work]]
    2. How B-Tree indexes work
    3. How LSM Tree & SSTable indexes work
    4. Indexes conclusion
2. Transactions
    1. ACID database transactions
    2. Read committed isolation
    3. Snapshot isolation
    4. Write Skew and Phantom writes
    5. Achieving ACID: Serial execution
3. Database Internals
    1. Two Phase Locking
    2. Serialisable Snapshot Isolation
    3. Column Oriented Storage
    4. Data Serialisation Frameworks
4. [[personal vault/System Design/High Level Design/Introduction to Replication\|Introduction to Replication]]
	1. Replication Strategies
		1. [[personal vault/System Design/High Level Design/Single Leader Replication\|Single Leader Replication]]
		2. [[personal vault/System Design/High Level Design/Multileader Replication\|Multileader Replication]]
			1. [[personal vault/System Design/High Level Design/Dealing with write conflicts\|Dealing with write conflicts]]
			2. [[personal vault/System Design/High Level Design/CRDT's - Conflict-Free Replicated Data Types\|CRDT's - Conflict-Free Replicated Data Types]]
		3. [[personal vault/System Design/High Level Design/Leaderless Replication\|Leaderless Replication]]
		4. [[personal vault/System Design/High Level Design/Replication Summarized\|Replication Summarized]]
6. [[personal vault/System Design/High Level Design/Introduction to Partitioning\|Introduction to Partitioning]]
	1. [[personal vault/System Design/High Level Design/Two phase commit - Distributed Transactions\|Two phase commit - Distributed Transactions]]
	2. [[personal vault/Data Structures and Algorithms/To be Revisited/Consistent Hashing\|Consistent Hashing]]
7. Consistency and Consensus
	1. [[personal vault/System Design/High Level Design/Linearisable Databases\|Linearisable Databases]]
	2. [[personal vault/System Design/High Level Design/Distributed Consensus - Raft Leader Election and Raft Writes\|Distributed Consensus - Raft Leader Election and Raft Writes]]
	3. [[personal vault/System Design/High Level Design/ZooKeeper - Coordination Services\|ZooKeeper - Coordination Services]] - Wraps up consensus
8. Databases
	1. [[personal vault/System Design/High Level Design/Relational vs Non Relational Databases\|Relational vs Non Relational Databases]]
		1. [[personal vault/System Design/High Level Design/MySQL vs PostgreSQL\|MySQL vs PostgreSQL]]
		2. [[personal vault/System Design/High Level Design/VoltDb\|VoltDb]]
		3. [[personal vault/System Design/High Level Design/Google Spanner\|Google Spanner]]
		4. [[personal vault/System Design/High Level Design/MongoDB vs Apache Cassandra\|MongoDB vs Apache Cassandra]]
		5. 
9. 
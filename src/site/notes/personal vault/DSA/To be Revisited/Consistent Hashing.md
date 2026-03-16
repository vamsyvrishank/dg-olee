---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/consistent-hashing/"}
---


Consistent hashing is a distributed hashing technique used to distribute data across a cluster of servers or nodes in a way that minimizes the amount of data that needs to be redistributed when nodes are added or removed from the cluster. It is particularly useful in distributed systems to achieve scalability and fault tolerance


Blog :  https://highscalability.com/consistent-hashing-algorithm/
Video : https://www.youtube.com/watch?v=4kd40gEKaLM

Uses of Consistent Hashing : Primary use : evenly distribution of data across multiple nodes in partition

### 1. **Distributed Databases**

**Examples**: Apache Cassandra, Amazon DynamoDB

- **Usage**: Consistent hashing is used to distribute data across multiple database nodes, ensuring that data is evenly distributed and that the system can scale horizontally by adding more nodes with minimal data reshuffling.
- **Benefit**: Minimizes data movement when nodes are added or removed, which helps in maintaining a high level of performance and availability.

### 2. **Distributed Caching Systems**

**Examples**: Memcached, Redis

- **Usage**: Consistent hashing helps in distributing cached data across multiple cache servers. This allows easy scaling by adding or removing servers without having to invalidate a significant portion of the cache.
- **Benefit**: Ensures that the cache remains effective and that only a minimal amount of data needs to be redistributed during scaling operations.


### 3. **Content Delivery Networks (CDNs)**


**Examples**: Akamai, Cloudflare

- **Usage**: CDNs use consistent hashing to distribute content across a network of servers located in different geographical regions. This helps in delivering content from the nearest server to the end user, reducing latency.
- **Benefit**: Provides efficient load balancing and redundancy, ensuring high availability and low latency for content delivery.

### 4. **Load Balancing**

**Examples**: Nginx, HAProxy

- **Usage**: In load balancing, consistent hashing is used to route incoming requests to servers. This ensures that requests for the same content are consistently directed to the same server, which can be crucial for session-based applications.
- **Benefit**: Provides efficient routing, reduces server load, and helps in maintaining session continuity.

### 5. **Peer-to-Peer Networks**

**Examples**: BitTorrent, Chord (a distributed hash table)

- **Usage**: Consistent hashing helps in distributing and locating data among peers in a peer-to-peer network. Each peer is assigned a portion of the hash space, and data is stored in the peer responsible for that portion.
- **Benefit**: Enables efficient and decentralized data storage and retrieval, enhancing the scalability and robustness of the network.


### 6. **Distributed Message Queues**

**Examples**: Apache Kafka, RabbitMQ

- **Usage**: Consistent hashing is used to partition messages among different queues or brokers, ensuring that related messages are consistently sent to the same partition.
- **Benefit**: Facilitates efficient message routing and processing, maintaining order and reducing the complexity of managing large volumes of messages.

### 7. **Key-Value Stores**

**Examples**: Amazon DynamoDB, Riak

- **Usage**: In key-value stores, consistent hashing helps in distributing keys across a set of nodes. This ensures that the data associated with each key is stored and retrieved from the appropriate node.
- **Benefit**: Ensures scalability and high availability, as adding or removing nodes requires minimal redistribution of keys.

### 8. **Microservices Architectures**

**Examples**: Kubernetes, Docker Swarm

- **Usage**: Consistent hashing can be used to route requests to microservices instances. This helps in maintaining session stickiness and balancing the load among instances.
- **Benefit**: Improves scalability and reliability of microservices by efficiently distributing workloads.

### 9. **Distributed File Systems**

**Examples**: Google File System (GFS), Hadoop Distributed File System (HDFS)

- **Usage**: Consistent hashing helps in distributing files and data blocks across multiple storage nodes. This allows for efficient data retrieval and balancing of storage loads.
- **Benefit**: Ensures data redundancy, fault tolerance, and efficient use of storage resources.
### 10. **Distributed Search Engines**

**Examples**: Elasticsearch, Apache Solr

- **Usage**: Consistent hashing helps in distributing search indices across multiple nodes. This allows for efficient query handling and balancing the search load.
- **Benefit**: Provides high availability and fast search capabilities by distributing the data and search queries.


## Modulus Operator

![[Pasted image 20240618220524.png \| 800]]
![[Pasted image 20240618220636.png  \| 400]]



### The hashing Problem

The problem arises when the servers go down , the value of n changes, this changes the server id generated for the same key to be different.
When we are performing any read , the data would be initially present in some other node and when any node would have gone down, the updated data would have come to other node. This causes inconsistency.
What if we need to add more servers ? Same problem, data needs to be moved which is costly. 


## Consistent Hashing

We use a conceptual hash ring. let us assume we have the ring with total no of hash values 'n' , and use node's ip address to calculate the hash value and place it on the ring
![[Pasted image 20240618221138.png \| 500]]

Similarly keys are hashed using the same hash function as well.

![Pasted image 20240618222801.png\|500](/img/user/personal%20vault/Attachments/Pasted%20image%2020240618222801.png)
 Each key goes in the clockwise direction to the respective server. with this only a fraction of keys needs to be shifted. lets say if s1 goes down , k1 will go to s2 in a clockwise manner. 
 (removing an existing node) - scaling down 
 (adding a new node) - scaling up

### Uniform Distribution Problem

Even with consistent hashing we might end with irregular distribution of data.
![[Pasted image 20240618223148.png \| 500]]
![[Pasted image 20240618223233.png \| 400]]
server 1 has become a bottle neck here.


## Virtual Nodes ( Solution ) 

![[Pasted image 20240618223332.png \| 500]]
Instead of applying single hash function we can apply multiple hash functions to the same node's IP

![Pasted image 20240618223436.png\|500](/img/user/personal%20vault/Attachments/Pasted%20image%2020240618223436.png)

With key we will only use 1 hash function.

![Pasted image 20240618223544.png\| 500](/img/user/personal%20vault/Attachments/Pasted%20image%2020240618223544.png)
The load of the request is more uniform now.
Moreover if a certain node has more hardware capacity it can have more virtual nodes as well.
![Pasted image 20240618223644.png\|500](/img/user/personal%20vault/Attachments/Pasted%20image%2020240618223644.png)


In conclusion as the no of virtual nodes increases, the distribution of keys become more balanced. ( with 100 VN we might get a var of 10%)


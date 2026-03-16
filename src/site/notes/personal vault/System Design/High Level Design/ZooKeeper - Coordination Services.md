---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/zoo-keeper-coordination-services/"}
---



**Apache ZooKeeper** is a *distributed coordination service* designed to manage and coordinate large-scale distributed systems. It provides a simple and reliable set of primitives that allow distributed applications to coordinate with each other, maintain configuration information, name services, and provide distributed synchronization. ZooKeeper is commonly used in distributed systems to manage configurations, leader elections, and other coordination tasks.

Coordination services are layers that are build on top of some distributed consensus algorithm like raft in order to help us with all of the configurations in a distributed system.
what configurations ? 

- IP for servers ( Loadbalancers , CDN)
- Replication Schemas ( leader info , where are the db located etc )
- Partitioning Breakdown ( what node , what keys , what hash keys , hash functions )

Recall : Coordination is slow , but sometimes we need it.

A coordination service is a key-value store that allows us to store this data in a reliable way.
Example : Zookeeper , Etcd

We discussed how writes and leader election works in consensus 

### How Reads works ? 

![[Pasted image 20240810213052.png \| 600]]


Zookeeper makes sure to sync the followers so reads are linearizable

![[Pasted image 20240810213302.png \| 600]]


### Conclusion :

Too slow to be used for application data , like Facebook , Twitter , if we send all writes to a single leader and bascially that has to effectively go through some sort of 2 Phase commit then we will run into performance problems.

Only key pieces of configuration for your backend that need to be correct.
Build on top of consensus algorithm to maintain linearizability.


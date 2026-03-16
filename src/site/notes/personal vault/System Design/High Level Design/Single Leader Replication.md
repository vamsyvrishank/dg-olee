---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/single-leader-replication/"}
---


## Replication Strategies

### Single Leader Replication 

This is a replication strategy also called primary-replica or master-slave replication , one node ( leader / primary ) is designated authoritative source of data. and all write operations are directed to this leader. The leader then propagates these changes to replicas/slaves

![[Pasted image 20240720160843.png \| 600]]

Lets assume we are using Asynchronous replication : that is what happens 99% of time.

Benefits:
	- Many copies of data = increased durability
	- Increased Read throughput - Why ? since we have multiple nodes to read from.

Failure Scenarios:
	Follower goes down :
	- ![[Pasted image 20240720161519.png \| 600]]
	- We have replication logs that , then leader would send the data and the follower would receive the data from the replication log.

- Leader Goes Down : 
	Case 1 
	-![[Pasted image 20240720161754.png \| 500]]
	It might be possible that the follower guy has bad internet and the leader might be still up.
	
   Case 2
   ![[Pasted image 20240720161942.png \| 500]]
   If leader actually goes down, the follower becomes the leader , but that guy does not have the updated data , since it is async the follower might assume that he has all the data.
   
    Case 3
    ![[Pasted image 20240720162257.png \| 500]]
    Two leaders are there now , causes Split Brain. causes problems.

WE NEED DISTRIBUTED CONSENSUS ! 

Conclusion :
- Simple easy to modify existing backend to add replicas
- Easy fix if follower goes down
- More read throughput
What if we want more write throughput ? next topic.


Key Characteristics:
- Single points of writes
- Read Scalability
- Consistency
- Simplified Conflict Resolution
Advantages:
- Simplicity
- Strong consistency for writes
- Improved read scalability
- Fault Tolerance : Followers can take over if the leader fails, improving system availability. This typically involves a leader election process to promote a follower to a leader.
Disadvantages:
- Leader Bottlenecks - single only 1 node of leader
- Replication Lag - Followers lag behind the leader
- Single Point of failure - leader
- Complex Failovers - Having leader failures and ensuring seamless failover to new leader can be complex
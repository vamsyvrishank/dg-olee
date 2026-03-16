---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/replication-summarized/"}
---


Point of replication : 

1. Durability
2. Throughput 
3. Geography

Multiple reads and multiple writes 


### Single Leader Replication

![[Pasted image 20240803143122.png \| 500]]
this will be eventually consistent.
very impractical though.

if leader goes down , you can designate a follower to become the leader , but when the leader comes back up -  causes split brain -> distributed consensus do solve this though.

### Multi - Leader Replication

![[Pasted image 20240803143409.png \| 500]]

write conflicts : 
we cannot use timestamp , clocks are bad , unrelaible , clock drift 
we can use version vectors for concurrent writes 
	- either to store both of them as siblings and user can decide.
	- or use CRDT where db automatically merge


### LeaderLess Replication

![[Pasted image 20240803143711.png \| 500]]

read the highest version number.

 - Read Repair
 - Anti entropy ( merkle trees )
 these are consistent background due to above 
 db send changes through a data structure using merkle trees.


high read latency -> since we need to read from multiple tables
write conflicts : write fails and stuff 

it is because of write conflicts that quorum read/writes are not perfect
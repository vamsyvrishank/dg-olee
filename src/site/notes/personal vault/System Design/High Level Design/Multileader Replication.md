---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/multileader-replication/"}
---



![[Pasted image 20240720165057.png \| 800]]

we have more write throughput.
Europe guy can write to Europe leader.

### Circle Topology.
All folks here are leaders.

![[Pasted image 20240720165324.png \|500]]

if one guy goes down, the dependent person is screwed. what can we do to make this better ? 

### Start Topology
]![[Pasted image 20240720165439.png \| 500]]

it is good as long as centre node is not dead. now all the db cannot communicate to each other.

### All to All topology

![[Pasted image 20240720165544.png \| 500]]

 Writes get out of order , causality issues , this is fixable though.

### Write Conflicts

![[Pasted image 20240721130036.png \| 600]]

How do we know the true leader.
Sol : All writes to the same key goes to same replica but it is also going to limit the write throughput.


last write wins ? can we use that ? 
![[Pasted image 20240721130335.png \| 600]]

---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/dealing-with-write-conflicts/"}
---


we talked about multileader replication , instead of writing to one place we can acutally write to multiple places,  this increases write throughput. This gives arise to write conflicts.

We will talk about detecting concurrent writes , how this is better than "last write wins"

![[Pasted image 20240721130951.png \| 600]]

We can identify concurrent writes by assigning them a timestamp , but timestamps themselves are not very reliable.

### Build a distributed Counter
(using a multileader replication setup)

![[Pasted image 20240721132239.png  \| 800]]

both of the parties here would not know their values , ideal ans should be 8.


Version - 2: 
Concept of version vector : 

![Pasted image 20240803110546.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803110546.png)

This time instead of storing the counter , we will store the increments from the leader node.
( Each entry of the version vector represents how many writes from each leader has been accounted for on that node)

Eventually these databases send their writes to each other. after merging we get value of counter as 8.

in the next case , if we increment by 1. version vector becomes (4,5) -> 9.
when data is sent to the second leader. we add the delta to version vector.


When writes are concurrent in all-all leader replication ? 

![Pasted image 20240803111246.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803111246.png)

How to deal with those concurrent writes ? 

Option 1 : Store siblings , have user deicide later.
![[Pasted image 20240803113653.png \| 500]]

(0,1) and (1,0) are concurrent since we cannot compare now.

Options 2 : DB automatically do it.
This is the premise behind CRDT's ,

Conflict - Free Replicated Data Types

The point of these is to be eventually consistent , you may have concurrent writes to different leaders , but eventually when they are passed around to the leader nodes , the leaders will agree on the value ( like how they both agreed on the count of 8 in our original example)

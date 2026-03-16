---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/crdt-s-conflict-free-replicated-data-types/"}
---


Oh no we have write conflict , what do we do ? 

Options:
1. Last Write Wins - not very reliable , we cant trust timstamps.
2.  Store siblings and let a user decide - version vectors.
3. Have the data base merge them automatically ( CRDT)

![[Pasted image 20240803114353.png \| 500]]

Where CRDT are used : 
1. Riak ( mulitleader / leaderless distributed key-value store)
2. Redis ( in memory db used for caching ) ( sets in redis enterprise)


Operational CRDTs:
![[Pasted image 20240803114540.png \| 500]]

Instead of sending the whole vector across the leaders , we send only the operations.

Downsides of Operational CRDT's:
![Pasted image 20240803114907.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803114907.png)

writes are causally related , if we have an operational CRDT's we need a causally consistent broadcasting system where messages do not get dropped or duplicated.
Because these operations are not known as idempotent ( do same thing many times over and no difference)

State-Based CRDT's:
![Pasted image 20240803115136.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803115136.png)

What does this really work well with ? 

It works with Gossip Protocol
![Pasted image 20240803115526.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803115526.png)

All CRDT state updates are going to propagated to the whole system. since operations are idempotent it works well even if it receives duplicates.


Data Structures via CRDT and use well in multileader replication setup : 
![Pasted image 20240803115710.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803115710.png)


Lets build sets : 

![Pasted image 20240803115936.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803115936.png)

![Pasted image 20240803120100.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803120100.png)
why ? becuase we receive multiple removes due to the fact that we have designed our architecture based on idempotency of operations.


Finally : Sequence CRDT - build consistent list, Very hard cuz element are ordered.
very tough in practice to implement.

examples : Colab Google docs , vs code colab real time.


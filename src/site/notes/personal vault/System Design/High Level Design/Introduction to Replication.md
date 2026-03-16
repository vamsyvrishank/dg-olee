---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/introduction-to-replication/"}
---


State Reads : reading old data because systems are highly available but not consitent

- Strong Replication - Strong Consistency
- Asynchronous Replication - Eventual Consistency

In strong consistency it is not possible to read stale data ( old data ) -> because strong consistency

Modern web applications use Eventual Consistency is the norm.


### How we replicate ? 

![[Pasted image 20240720143055.png \| 600]]

SQL - we cannot insert the data through SQL , copying them directly over to replica 
	problems ? Non deterministic 
WAL - we have memory address , but not all of replica would be running the same data softwares

<mark class="hltr-green">Replication Log (Often done in practice) - Which ID do we need to write for.</mark>


###  Eventual Consistency ( dealing with Stale Reads - Make your app better)

Problem : it is eventual.
Strong consistency is too big of a price to pay.

1. **Reading your own writes**
	![[Pasted image 20240720144302.png \| 500]]

	Well if we are making the write , read only from the db you wrote to ? or put a time stamp at t=100 , then we can go to any db and ask have you seen any writes from t=100 , then it mean i can read from you.
		Time stamps aren't too reliable , in majority it is good enough.
	
2. **Monotonic Writes and Reads**
		Monotonic writes are consistency guarantee in a distributed systems and data replication that ensure the order of writes by a single process preserved across all replicas.
			Write Ordering :  Monotonic writes ensure that if a process issues a write W1 followed by another write W2, any replica that has applied W2 must have already applied W1.
		Similarly for reads , we need to make sure that we are reading in correct order the writes were sent , here jordan is suggesting we use the same replica for everything , since the writes are sequential for a single replica , we can ensure read consistency
		
3. Consistent Prefix Reads
		Consistent Prefix reads are consistency guarantee in a distributed system that ensure 
		![[Pasted image 20240720155208.png \| 500]]
		Consistent prefix reads are a consistency guarantee in distributed systems that ensure any read operation reflects a prefix of all the previous writes.
		Fix :  Keep track of causal dependencies of writes.
		This means that if a sequence of write operations is performed, any read operation will see these writes in the correct order, up to some point in the sequence. However, the read might not include the most recent writes, but it will never show writes out of order.
	Key Characteristics : 
		- Ordered Reads : maintains order of write operations when reads are performed.
		- Partial Visibility : While all the writes are in correct order , not all writes are visible ( eventual consistency ) -> data is always consistent and ordered.
    Examples : 
	    - Social Media Feed : When a user reads their feed, consistent prefix reads ensure that if they see a post, they will also see any earlier posts in the correct order
	    - Distributed Database : In a distributed database, if multiple updates are made to a record, consistent prefix reads guarantee that any read operation will return the updates in the correct order. 1. - For example, if an item’s price is first set to $10 and then updated to $20, a read might show $10 or $20, but not a price that skips the first update.
	 Importance and Use cases:
		- Eventual Consistency System
		- User Experience
		- Financial Systems : consistent prefix reads ensure that users see a consistent and ordered history of transactions
	 Implementation :
		- Write Propagation : Ensure that writes are propagated in a way that maintains their order. This might involve using ordered logs or version vectors to track write operations.
		- Read from single Replica
		- Metadata tracking : use metadata such as timestamps or sequence numbers.
	 Challenges :
		- Latency : since the writes are not immediately visible
		- Complexity : careful co-ordination of write and read operations.
		- Availability and Consistency : There is a trade-off between availability and consistency, particularly in the presence of network partitions. Ensuring a consistent prefix might mean sacrificing some availability




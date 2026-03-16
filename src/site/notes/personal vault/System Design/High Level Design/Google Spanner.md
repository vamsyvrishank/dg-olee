---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/google-spanner/"}
---




**Main Problem: Distributed Reads in a RDMS**

- In a distributed relational database management system (RDBMS), ensuring causally consistent reads across multiple nodes is a significant challenge. Causal consistency means that if one transaction depends on the result of another, the system must ensure that the read reflects this order, even when distributed across different nodes


### Traditional Approaches:

1. **Snapshots for Consistency**:
    
    - **Snapshot Isolation**: Typically, snapshots can be used to provide consistent views of data by taking a "picture" of the database at a specific point in time. However, without a centralized system, it's difficult to coordinate which snapshots to use across distributed nodes, leading to potential inconsistencies.
2. **2PL (Two-Phase Locking) with Shared Locks for Reads**:
    
    - **Shared Locks**: Another approach is to use shared locks during reads to prevent any modifications to the data while it's being read. However, this can severely slow down the system, as acquiring locks for reads blocks new writes, reducing concurrency and overall system performance.

### How Spanner Addresses Causally Consistent Distributed Reads:

- **TrueTime API**: Spanner introduces the TrueTime API, a globally synchronized clock that provides tight bounds on time across all nodes in the system. This allows Spanner to assign globally consistent timestamps to transactions.
    
- **Causally Consistent Reads Without Locking**:
    
    - **Timestamp Ordering**: In Spanner, every write is assigned a timestamp using TrueTime. Any write that depends on another write will automatically receive a greater timestamp, ensuring a consistent ordering of events.
    - **Lock-Free Consistency**: By using these timestamps, Spanner allows for causally consistent reads across distributed nodes without requiring any locking mechanism. When a read operation occurs, Spanner ensures that it reads from the appropriate version of the data (based on the timestamp) that respects the causal order of transactions.
- **Global Consistency**: This approach not only maintains causal consistency but also ensures that distributed reads are consistent across the entire system, regardless of the geographic distribution of data.


### Conclusion:

Spanner achieves lock-free distributed reads, enabling high performance and strong consistency in a globally distributed environment. However, this capability comes at a significant cost: each database node requires GPS-synchronized clocks to maintain accurate global timestamps, making Spanner much more expensive compared to typical open-source systems. This advanced infrastructure is key to Spanner’s unique strengths but adds a layer of complexity and cost that might not be necessary for all applications.
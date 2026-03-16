---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/linearisable-databases/"}
---


A linearizable database ensures that all operations appear to occur atomically and in a sequential order. *Linearizability is a strong  consistency model* that guarantees that once a write operation completes, all subsequent read operations will reflect that write. This model is also known as "strong consistency" or "atomic consistency."


The writes are ordered , so our reads never go back in time.



### Examples of Linearizable Storage Systems

1. **Google Spanner:**
    - A globally distributed database that provides linearizable consistency through the use of synchronized clocks and a two-phase commit protocol.
2. **CockroachDB:**
    - An open-source distributed SQL database designed to offer linearizable consistency, leveraging a consensus algorithm (such as Raft) for coordination.
3. **Apache ZooKeeper:**
    - A coordination service for distributed systems that offers linearisable reads and writes, often used for configuration management and synchronization.

### How do we order our writes ? 

Single Leader Replication - replication log
and this log is sent from leader to followers.

Multi-leader /Leaderless replication ?  - version vectors / Lamport clocks

![[Pasted image 20240807155438.png \| 500]]
o(n) space - n is the number of replica


### Lamport Clocks

Lamport clocks are a logical clock system used in distributed systems to order events. They provide a way to determine the *partial ordering* of events and help establish a *happened-before* relationship. Lamport clocks are particularly useful in scenarios where physical clocks cannot be perfectly synchronized.

### How Lamport Clocks Work

1. **Initialization:** Each process in the system has a counter (logical clock) initialized to zero.
2. **Event Occurrence:** Each time an event occurs within a process, the process increments its logical clock by one.
3. **Message Sending:** When a process sends a message, it includes its current logical clock value (timestamp) with the message.
4. **Message Receiving:** When a process receives a message, it updates its logical clock to be the maximum of its current clock value and the received timestamp, and then increments its logical clock by one.

## Problem ? 

Lamport clocks help in establishing a causal relationship between events but do not guarantee linearizability on their own. Linearizability requires a total order of operations that respects real-time ordering, while Lamport clocks only provide a partial order.

#### Multi-Leader Replication

- **Scenario:** Multiple leaders can accept write operations simultaneously, and these writes need to be propagated and replicated across all nodes.
- **Challenge:** Ensuring a consistent order of operations across all leaders.
- **Role of Lamport Clocks:** They help in establishing causal relationships between writes, but additional mechanisms (such as consensus protocols) are required to enforce a total order and achieve linearizability.
#### Leaderless Replication

- **Scenario:** Any node can accept writes, and the writes are propagated to other nodes.
- **Challenge:** Handling conflicting writes and ensuring all nodes converge to the same state.
- **Role of Lamport Clocks:** They assist in determining the order of conflicting writes, but again, additional conflict resolution strategies (like vector clocks or consensus algorithms) are necessary to achieve linearizability.

what if we read from other nodes while the updates are being propagated ? 


###  How to Achieve Linearizability ? 

- **Consensus Algorithms (e.g., Paxos, Raft):** These algorithms can be used in conjunction with Lamport clocks to ensure a total order of operations. They provide the necessary synchronisation and agreement among nodes to maintain linearizability.
- **Synchronized Clocks (Google Spanner):** Spanner uses tightly synchronized physical clocks (TrueTime) to achieve global consistency and linearizability.

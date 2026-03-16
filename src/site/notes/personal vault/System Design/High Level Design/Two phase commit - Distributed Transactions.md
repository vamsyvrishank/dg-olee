---
{"dg-publish":true,"permalink":"/personal-vault/system-design/high-level-design/two-phase-commit-distributed-transactions/"}
---


![Pasted image 20240803195833.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803195833.png)

We need the functionality of cross partition writes.

The Two-Phase Commit (2PC) protocol is a consensus algorithm used to ensure all nodes in a distributed system agree on a transaction's outcome, either commit or abort, to maintain consistency across the system.

### How does 2PC works ? 

1. **Prepare Phase**:
    
    - The coordinator node sends a `prepare` request to all participant nodes.
    - Each participant performs necessary checks and operations to prepare for the transaction.
    - Participants respond with a `vote` to commit or abort.
2. **Commit Phase**:
    - If all participants vote to commit, the coordinator sends a `commit` message to all participants.
    - If any participant votes to abort, the coordinator sends an `abort` message to all participants.
    - Participants execute the commit or abort as instructed.

![[Pasted image 20240803200850.png \| 500]]


if both say ok , then it maintains a WAL in commit log and even though it crashes , it can read from the commit log and do the operation for commit.

![[Pasted image 20240803201841.png \| 500]]

### Pros

- **Consistency**: Ensures that all nodes have a consistent view of the transaction outcome.
- **Atomicity**: Guarantees that a transaction is either fully completed or fully rolled back.

### Cons

- **Blocking**: If the coordinator fails, participants may be left in a locked state until recovery.
- **Latency**: Involves multiple communication steps, which can increase the transaction's overall latency.
- **Complexity**: Implementing 2PC requires additional logic for handling different failure scenarios.

![Pasted image 20240803203630.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803203630.png)


Conclusion : 


![Pasted image 20240803203706.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020240803203706.png)


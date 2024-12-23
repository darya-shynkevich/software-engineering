> Saga design pattern is often used to achieve data consistency and reliability in distributed systems.

Saga is often used to maintain **data consistency** in a **microservices** architecture. And it **prevents tight coupling**.

!!! Saga doesn't acquire locks on remote resources and uses compensation to handle failures. Thus it **scales well.**

![](Pasted%20image%2020240708104942.png)

**Orchestrator**: manages sub-transactions & interacts with log
**Log**: stores the state of each sub-transaction
**Compensating transaction**: idempotent for retries & revert failure

## Architecture

### 1. Divide & Conquer

It splits a [transaction](1.%20Software%20Engineering/3.%20Database/OTLP/SQL/3.%20Transactions/_Base.md) into sub-transactions.

And assigns a separate sub-transaction to each database partition.

That means it still looks like a single transaction.

![](Pasted%20image%2020240708104542.png)
Besides a revert action is available for each sub-transaction - **compensating** **transaction**. It’s like a correction and not an undo. For example, canceling a hotel booking instead of removing it.
- The compensating transactions get executed only if a sub-transaction fails.
- The compensating transactions must be [idempotent](https://newsletter.systemdesign.one/p/idempotent-api), so retries are possible without side effects.

### 2. Interacting with Database Partitions

They set up a separate service to manage sub-transactions and called it **Orchestrator**:
- Control sub-transactions
- Apply compensating transactions if needed
![](Pasted%20image%2020240708104700.png)
### 3. State Information

It’s necessary to store each sub-transaction's state (start & end) outside Orchestrator. Otherwise it will become a single point of failure.

So they use a durable and distributed **log**:
- Track if a sub-transaction failed
- Find compensating transactions that must be executed
- Track the state of compensating transactions
- Keep the orchestrator stateless
- Recover from failures
![](Pasted%20image%2020240708104752.png)
# References:

1. [Understanding the SAGA Pattern: A Guide to Implementing Reliable Distributed Transactions](https://jinlow.medium.com/understanding-the-saga-pattern-a-guide-to-implementing-reliable-distributed-transactions-a6772d76d194)
2. [Applying the Saga Pattern • Caitie McCaffrey • GOTO 2015](https://www.youtube.com/watch?v=xDuwrtwYHu8) (video)
3. [Reference 6: A Saga on Sagas](https://learn.microsoft.com/en-us/previous-versions/msp-n-p/jj591569(v=pandp.10)?redirectedfrom=MSDN)
4. ~~[How Halo Scaled to 11.6 Million Users Using the Saga Design Pattern](https://newsletter.systemdesign.one/p/saga-design-pattern?utm_source=substack&publication_id=1511845&post_id=145900943&utm_medium=email&utm_content=share&utm_campaign=email-share&triggerShare=true&isFreemail=true&r=1vxw4z&triedRedirect=true)~~

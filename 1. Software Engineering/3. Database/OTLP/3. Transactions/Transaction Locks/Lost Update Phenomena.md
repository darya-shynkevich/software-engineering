Always happens inside the transaction

Occurs when two different transactions update value at the same time. The first transaction updates a record and the second transaction updates the same record again, which nullifies the update of the first transaction. As the update by the first transaction is lost this concurrency problem is known as the lost update problem.

## How to prevent?

### 1.  Increase Transaction Isolation Level

Set isolation level to Repeatable Read
### 2. [[Transaction Locks/Optimistic and Pessimistic]]

### 3. Atomic write operations





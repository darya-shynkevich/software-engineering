In computer science, **ACID** (atomicity, consistency, isolation, durability) is a set of properties of database transactions intended to guarantee data validity despite errors, power failures, and other mishaps.

- A guarantee of ==**Atomicity**== prevents updates to the database from occurring only partially, which can cause more significant problems than rejecting the whole series outright.
- ==**Consistency**== guarantees that transaction can move database from one valid state to the next. This ensures these all adhere to all defined database rules. Also preventing corruption by illegal transaction. 
- ==**Isolation**== determines how a particular action is shown to other concurrent system users.
- ==**Durability**== is the property that guarantees that transactions that have been committed will survive permanently.

[[Transaction Loks]]

[[Transaction concurrency]]

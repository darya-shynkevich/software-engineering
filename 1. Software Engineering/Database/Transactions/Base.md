Транзакция - действие / серия действий которые осущетствляют доступ или изменение содержимого БД
1. a collection of queries
2. one unit of work

Транзакции переводят БД из одного согласованного состояния в другое.

In computer science, **ACID** (atomicity, consistency, isolation, durability) is a set of properties of database transactions intended to guarantee data validity despite errors, power failures, and other mishaps.

- A guarantee of ==**Atomicity**== prevents updates to the database from occurring only partially, which can cause more significant problems than rejecting the whole series outright. 
  All or nothing: all queries must succeed. If one fails, the rest should rollback.
- ==**[Isolation](Isolation.md)**== determines how a particular action is shown to other concurrent system users.
- ==**Consistency**== guarantees that transaction can move database from one valid state to the next. This ensures these all adhere to all defined database rules. Also preventing corruption by illegal transaction. 
		1. Consistency in Data
			1. Defined by the user
			2. Referential integrity (FK)
			3. Atomicity
			4. [Isolation](Isolation.md)
		2. Consistency in Reads
			1. If a transaction committed a change will a new transaction immediately see the change?
			2. Relational and NoSQL DBs suffer from this. Because even relational DBs are not consistent in reads (example: adding more replicas)
			3. [Eventual Consistency](Eventual%20Consistency)
- ==**Durability**== is the property that guarantees that transactions that have been committed will survive permanently.
	- Redis, Memcached are not durable DBs
# [Isolation](Isolation.md) Levels

# [Transaction Loks](Transaction%20Loks.md)

# [Transaction concurrency](Transaction%20concurrency.md)

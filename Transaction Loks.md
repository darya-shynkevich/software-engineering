
# Be pessimistic or optimistic

**Pessimistic lock**: the idea is that before I want to touch this system resource (not have to be database records), it could be a file or network port, ***I want to assume that everyone else who uses this resource will affect my work***. so I want to lock them to prevent others from making changes. 

**Optimistic lock** is another way wrong, which means ***I am okay to share the resource with other processes who need it***, as long as not affecting my job, ***when conflict happens we can resolve it on the spot***.

# Shared or Exclusive

I was also confused by the word “**shared lock**”, if it is shared, why lock? so just remember that `Shared` is for reading => I allow another process to read this resource.

**Exclusive lock** is easy to understand: I am using it, locking it, and no one should be able to access (read or write) it.

# Range Locks

Range locks are best described by illustrating all the possible lock levels.

1. ***Serialized Database Access*** — *Making the database run queries one by one — terrible concurrency, the highest level of consistency, though.*
2. ***Table Lock*** — *lock the table for your transaction with slightly better concurrency, but concurrent table writes are still slowed.*
3. ***Row Lock*** — *Locks the row you are working on even better than table locks, but if multiple transactions need this row, they will need to wait.*

***Range locks*** are between the last two levels of locks; *they lock the range of values captured by the transaction and don't allow inserts or updates within the range captured by the transaction.*

# Isolation Levels

The SQL standard defines 4 standard isolation levels these can and should be configured globally (insidious things can happen if we can't reliably reason about isolation levels).

![[Pasted image 20230605134755.png]]


#### READ UNCOMMITTED

The _READ UNCOMMITTED_ isolation level doesn't maintain any transaction locking and can see uncommitted data as it happens, leading to dirty reads. The stuff of nightmares... in some systems.
##### Dirty reads

*A dirty read occurs when you perform a read, and another transaction updates the same row but doesn't commit the work, you perform another read, and you can access the uncommitted (dirty) value, which isn't a durable state change and is inconsistent with the state of the database.*

![[Pasted image 20230605134357.png]]

#### READ COMMITTED

Each read creates its own consistent (committed) snapshot of time. ==*As a result, this isolation mode is susceptible to phantom reads if we execute multiple reads within the same transaction.*==

1. Read only committed data
2. Lock VISIBLE recored when update:
	1. the record exist in the DB
	2. the record matched the “where” condition

Example: 

We have rows with ids 1, 2, 4 in the `table`

```SQL
select * from the table where id>2
```

visible record is 4 (3 is not included because it doesn't exist in the database) => susceptible for *Phantom reads* because it doesn’t lock the “Gaps” between records (including the last record to infinity)
##### **Phantom reads**

*Phantom reads are another committed read phenomena, which occurs when you are most commonly dealing with aggregates. For example, you ask for the number of customers in a specific transaction. Between the two subsequent reads, another customer signs up or deletes their account (committed), which results in you getting two different values if your database doesn't support range locks for these transactions.*

![[Pasted image 20230605134544.png]]


#### REPEATABLE READ

This isolation level ensures consistent reads within the transaction established by the first read. 

1. Read only committed data
2. Lock “Whatever records matched where condition” which could affect this transaction

It does have the minor data inconsistency while its locked to specific view of the database; ***keeping transactions short-lived as possible here is beneficial.***
##### Non-repeatable reads

*Non-repeatable reads occur if you cannot get a consistent view of the data between two subsequent reads during your transaction. In specific modes, concurrent database modification is possible, and there can be scenarios where the value you just read can be modified, resulting in a non-repeatable read.*

![[Pasted image 20230605134218.png]]

#### SERIALIZABLE

This operating mode can be the most restrictive and consistent since it allows only one query to be run at a time.

*==All types of reading phenomena are no longer possible since the database runs the queries one by one, transitioning from one stable state to the next.==* There is more nuance here, but more or less accurate.

_It is essential to note in this mode to have some retry mechanism since queries can fail due to concurrency issues._

Newer distributed databases take advantage of this isolation level for consistency guarantees. [CockroachDB](https://www.cockroachlabs.com/?ref=architecturenotes.co) is an example of such a database. Worth a look.

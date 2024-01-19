
To solve [Read Phenomenas](Read%20Phenomenas.md) people designed Isolation levels.

# Isolation Levels

The SQL standard defines 4 standard isolation levels these can and should be configured globally (insidious things can happen if we can't reliably reason about isolation levels).

![Pasted image 20231210225424](../../../../../_Attachments/Pasted%20image%2020231210225424.png)

## 1. READ UNCOMMITTED

No Isolation, any change from the outside is visible to the transaction

**Read Phenomena:** ***Dirty Reads***

## 2. READ COMMITTED

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

visible record is 4 (3 is not included because it doesn't exist in the database) => 

**Read Phenomena:** susceptible for ***Phantom reads*** because it doesn’t lock the “Gaps” between records (including the last record to infinity), ***Non-Repeatable*** reads (as ***each query*** in a transaction sees committed staff)

**The majority of DB is implemented this isolation level.** 
## 3. REPEATABLE READ

This isolation level ensures consistent reads within the transaction established by the first read. 

1. Read only committed data
2. Lock “Whatever records matched where condition” which could affect this transaction

It does have the minor data inconsistency while its locked to specific view of the database; ***keeping transactions short-lived as possible here is beneficial.***

**Read Phenomena:** ***Phantom reads*** (you can not control new rows, you can lock only existing rows)

Implementation:
1. [_Base](Transaction%20Locks/_Base.md)
2. [MVCC](MVCC.md) <-- some of the implementations can provide guarantees to get rid of Phantom Reads 

## 4. SERIALIZABLE

This operating mode can be the most restrictive and consistent since it ***allows only one query to be run at a time.***

*==All types of reading phenomena are no longer possible since the database runs the queries one by one, transitioning from one stable state to the next.==* There is more nuance here, but more or less accurate.

_It is essential to note in this mode to have some retry mechanism since queries can fail due to concurrency issues._

Newer distributed databases take advantage of this isolation level for consistency guarantees. [CockroachDB](https://www.cockroachlabs.com/?ref=architecturenotes.co) is an example of such a database. Worth a look.

In PostgreSQL it can be done like:

```SQL
brgin transaction isolation level serializable;
....
commit;
```

# References:

1. [Isolation Levels - part I: Introduction](https://dev.to/franckpachot/isolation-levels-part-i-introduction-bd5
2. [SQL Transaction Isolation Levels Explained](http://elliot.land/post/sql-transaction-isolation-levels-explained)
3. [Database Isolation Levels and Effects on Performance and Scalability](http://highscalability.com/blog/2011/2/10/database-isolation-levels-and-their-effects-on-performance-a.html)
4. [Dirty Read Problem - (Explained by Example)](https://www.youtube.com/watch?v=RxIDTbgdcpM&list=PLQnljOFTspQXOkIpdwjsMlVqkIffdqZ2K&index=80) (video)
5. [Phantom Read Phenomena - (Explained by Example)](https://www.youtube.com/watch?v=EA1sjQb_qpQ&list=PLQnljOFTspQXOkIpdwjsMlVqkIffdqZ2K&index=70) (video)
6. [Non-Repeatable Read (Fuzzy read) Phenomena - (Explained by Example)](https://www.youtube.com/watch?v=uTvQPSi_q1c&list=PLQnljOFTspQXOkIpdwjsMlVqkIffdqZ2K&index=72) (video)
7. [Read Committed DBMS Isolation Level - (Explained by Example)](https://www.youtube.com/watch?v=7cvU1Q0AJOU&list=PLQnljOFTspQXOkIpdwjsMlVqkIffdqZ2K&index=73) (video)
8. [Serializable vs Repeatable Read Isolation Level - When to use one over the other in Database System](https://www.youtube.com/watch?v=KoULlXKK1H8&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=64) (video)
9. [Phantom Reads in Postgres Explained Compared to Other DBMS](https://www.youtube.com/watch?v=MEAD5JNc_Dw&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=63) (video)
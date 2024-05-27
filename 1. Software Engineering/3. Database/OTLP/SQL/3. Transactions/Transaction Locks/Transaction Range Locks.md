Range locks are best described by illustrating all the possible lock levels.

The optimisation direction for database locks = Row locks > Gap Locks > Table Locks

1. ***Serialized Database Access*** — *Making the database run queries one by one — terrible concurrency, the highest level of consistency, though.*
2. ***Table Lock*** — *lock the table for your transaction with slightly better concurrency, but concurrent table writes are still slowed.*
3. ***Row Lock*** — *Locks the row you are working on even better than table locks, but if multiple transactions need this row, they will need to wait.*
	1. `ROWLOCK` — Offer the highest concurrency because they lock individual rows but may lead to deadlocks if not managed carefully.
	2. `UPDLOCK` — request update locks. It can also prevent other transactions from modifying the locked rows until the current transaction is completed.

***Range locks*** are between the last two levels of locks; *they lock the range of values captured by the transaction and don't allow inserts or updates within the range captured by the transaction.*


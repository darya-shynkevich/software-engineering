Range locks are best described by illustrating all the possible lock levels.

1. ***Serialized Database Access*** — *Making the database run queries one by one — terrible concurrency, the highest level of consistency, though.*
2. ***Table Lock*** — *lock the table for your transaction with slightly better concurrency, but concurrent table writes are still slowed.*
3. ***Row Lock*** — *Locks the row you are working on even better than table locks, but if multiple transactions need this row, they will need to wait.*

***Range locks*** are between the last two levels of locks; *they lock the range of values captured by the transaction and don't allow inserts or updates within the range captured by the transaction.*

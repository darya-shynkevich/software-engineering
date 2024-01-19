# [Optimistic and Pessimistic](Optimistic%20and%20Pessimistic.md)

# Shared or Exclusive

I was also confused by the word “**shared lock**”, if it is shared, why lock? so just remember that ***Shared is for reading*** => I allow another process to read this resource. *Позволяет транзакциям читать данные без ограничений; если какой-то транзакции нужно изменить данные => она будет ждать пока все совместные блокировки не будут сняты => применяет эксклюзивную блокировку и все остальные ждут.*
1. Reads: OK
2. Writes: NO

**Exclusive lock**: I am using it, locking it, and ***no one should be able to access*** (read or write) it => *Если транзакции А нужны данные, то она блокирует их. Если транзакции В нужны эти же данные => ждет пока А не снимет блокировку.*
	1. Reads: NO
	2. Writes: NO

To do WRITE you need to acquire an **Exclusive lock** => all READers will wait. 

Disadvantages:
1. Not good for concurrency

# Two Phase Locking

1. Используется в DB2, SQL Server
2. Работает хорошо за исключением отмен транзакций
3. все эксклюзивные блокировки снимаются только после завершения транзакций

Транзакции делятся на две фазы:
1. фаза подъема - транзакция может только применять блокировки, но не снимать их
2. фаза спада - только снимать блокировки 

Example with postgres 
1. T1: `select * from seats where id = 14 for update` <--- there will be an exclusive lock (Phase 1)
2. All other transactions that want exclusive locks will be released when the T1 will be committed.

## Deadlocks

A deadlock happens when two concurrent transactions cannot make progress because each one waits for the other to release a lock.

Because both transactions are in the lock acquisition phase, neither one releases a lock prior to acquiring the next one.

Если блокировка не снимается за какое-то определенное время => взаимная блокировка.
![Pasted image 20231218215650](../../../../../../_Attachments/Pasted%20image%2020231218215650.png)
##### Deadlock priority

Диспетчер транзакций выбирает какую транзакцию отменить:
1. которая изменила последний набор данных?
2. самую молодую?
3. которой требуется меньше времени на завершение?

As a rule of thumb, the database might choose to roll back the transaction with a lower rollback cost.

**Oracle**: the transaction that detected the deadlock is the one whose statement will be rolled back.

**SQL Server**: allows you to control which transaction is more likely to be rolled back during a deadlock situation via the [`DEADLOCK_PRIORITY`](https://docs.microsoft.com/en-us/sql/t-sql/statements/set-deadlock-priority-transact-sql) session variable. In case of a deadlock, the transaction will roll back, unless the other transaction has a lower deadlock priority value. If both transactions have the same priority value, then SQL Server rolls back the transaction with the least rollback cost.

**PostgreSQL**: as explained in the [documentation](https://www.postgresql.org/docs/12/explicit-locking.html), PostgreSQL does not guarantee which transaction is to be rolled back

**InnoDB**: tries to [roll back the transaction that modified the least number of records](https://bugs.mysql.com/bug.php?id=21293), as releasing fewer locks is less costly.
#### Предупреждение взаимных блокировок:

Алгоритм 1. Ожидание -> отмена
Более старые транзакции должны ожидать завершение новых. Иначе - отмена транзакции и перезапуск с той же временной меткой => самая новая => не отменяется

Алгоритм 2. Отмена -> ожидание
Более новые ожидают более старые. Если старая требует данные, которые захватила новая => новая отменяется
# [Transaction Range Locks](Transaction%20Range%20Locks.md)

# References:

1. ~~[Database Exclusive lock vs Shared Lock (Explained by Example)](!https://www.youtube.com/watch?v=b7razfltSFM&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=23)~~
2. ~~[Two Phase Locking Explained (2PL)](!https://www.youtube.com/watch?v=gv62vmvyy6s&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=20)~~
3. ~~[A beginner’s guide to database deadlock](!https://vladmihalcea.com/database-deadlock/)~~
4. ~~[Database Dead Locks Explained by Example](!https://www.youtube.com/watch?v=QzvVQ8vRDuM&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=30)~~
7. [Row-Level Database Locks Explained - (Read vs Exclusive)](https://www.youtube.com/watch?v=nuBi2XbHH18&list=PLQnljOFTspQWKPjGnVgA5oVIhNKJ5mDXg&index=33) (video)
8. [Управление блокировками](https://www.youtube.com/watch?v=MNwyw8mU7QY) (video)
9. [Блокировки в PostgreSQL](https://www.youtube.com/watch?v=_R2-IsKfsUU)
10. [Блокировки как механизм управления параллельными транзакциями: понятие блокировки, хранение блокировок, виды блокировок.](https://studfile.net/preview/2619005/page:26/)

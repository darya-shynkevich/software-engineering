
MVCC: maintain multiple snapshots of records using read-view to archive transaction concurrency safety.

## Read committed mode

![Pasted image 20231014190615](../../../../_Attachments/Pasted%20image%2020231014190615.png)

1. **Read-view:** a record including transaction ID list, creator transaction ID, max and min transaction ID
2. **Record snapshot:** record information transaction ID

Let’s see what happened in the above step by step.

1. `Thread1` started the transaction and made a query; a `read-review` record is created and labeled with `creator_trx_id=1`
2. Then `thread2` also started a transaction and made a query. similarly, a read-view record is created saved with `creator_trx_id=2`, and now in this record the `trx_list=[1,2]`
3. Now `thread2` update table set `name=’cde’` where `id=1`. what happened is that `thread2` *==**will clone the existing record then update its value (’abc’) with the current value (’cde’), and also link them together(roll pointer)**==*
4. `thread1` query again. A new read-view was created. so since its transaction ID is 1, and **the rule is to find the latest record in which the transaction ID is not in the current review’s active transaction list**. the match is recorded with trx_id=1. (trx_id=2 is still in the list so it has not been committed yet)
5. `thread2` committed.
6. `thread1` query again, new read-view created again. this time in its new read-view’s transaction ID list only got 1, so the latest record with `trx_id=2` is visible and fetched by thread1.

## Repeatable read mode

Then how about RR mode? only a little bit different from RC mode. so the difference is:

1. RC mode. Every time query will create a read-view
2. RR mode. only create a read-view in the first query (in the same transaction)

# References:

1. [How MVCC databases work internally](https://kousiknath.medium.com/how-mvcc-databases-work-internally-84a27a380283)
2. [MVCC-1. Изоляция](https://habr.com/ru/company/postgrespro/blog/442804/)
3. [Что должен, но не знает про конкуренцию в PostgreSQL каждый разработчик?](https://habr.com/ru/post/581854/)
4. [PostgreSQL Concurrency with MVCC](https://devcenter.heroku.com/articles/postgresql-concurrency)
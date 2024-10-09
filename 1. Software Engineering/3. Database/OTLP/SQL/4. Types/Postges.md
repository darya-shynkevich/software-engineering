! [A performance dashboard for Postgres](https://github.com/ankane/pghero)

! [PGTune - calculate configuration for PostgreSQL based on the maximum performance for a given hardware configuration](https://pgtune.leopard.in.ua/#/)

С перерывом в 2 года вернулся к работе с PostgreSQL и потребовалось быстро восстановить забытое и наверстать упущенное. Хочу поделиться книжками и ресурсами которые были мне полезны в изучении + актуализации знаний, возможно что то пригодится и вам. [Тред](https://twitter.com/_abstractart/status/1653435792214065182)

### Easily make lots of data

> Use `generate_series(m, n, [delta])`

- [Postgres doc on set returning functions](https://www.postgresql.org/docs/current/functions-srf.html)
- [Generating time series between two dates in Postgres](https://stackoverflow.com/questions/14113469/generating-time-series-between-two-dates-in-postgresql/46499873#46499873)
- [How to produce meaningful datasets using only SQL](https://medium.com/free-code-camp/how-to-produce-meaningful-datasets-using-only-sql-394c4781a5e0)

### `RETURNING`: Get back what you touched

```sql
# insert into person (name) values ('Groot') returning id;  
id  
----  
4  
(1 row)
```

- [Postgres docs on RETURNING](https://www.postgresql.org/docs/current/dml-returning.html)
- [Data-modifying statements in WITH](https://www.postgresql.org/docs/current/queries-with.html#QUERIES-WITH-MODIFYING) (where RETURNING is important)

### `WITH`, or Common Table Expressions

WITH queries allow you to decompose your query in more readable parts (which in Postgres ≥12 will likely execute as efficiently as less-readable equivalent queries), do advanced transformations across tables in a single statement or even serialize large object graphs in a single database round trip.

- [Postgres docs on WITH](https://www.postgresql.org/docs/12/queries-with.html)
- [Using Writeable CTEs to Improve Performance](https://omniti.com/seeds/writable-ctes-improve-performance.html) (via inserting to multiple tables in a single round-trip)
- [Postgres graph queries with billions of nodes](https://www.alibabacloud.com/blog/postgresql-graph-search-practices---10-billion-scale-graph-with-millisecond-response_595039) (Alibaba cloud)
- [Gentle intro on recursive SQL queries](https://towardsdatascience.com/recursive-sql-queries-with-postgresql-87e2a453f1b)
- [WITH-queries: Present and future](https://blog.crunchydata.com/blog/with-queries-present-future-common-table-expressions)

> **Common table expressions improved significantly in Postgres 12**. When reading about “[optimization barriers” and materialization and the dangers of CTEs](https://hakibenita.com/be-careful-with-cte-in-postgre-sql), consider whether it’s assuming Postgres ≤11 or other databases.

### Advisory locks

Advisory locks are explicit locks that last across transactions for as long as the connection that got the lock is alive.

If you need a Postgres connection to do your work, advisory locks can be a reasonable “leader election” mechanism as well. (For example, you may want to run multiple processes to sync data into Elasticsearch for high availability, but only need one of them to run at any moment)

- [Postgres docs on “explicit locking”, or advisory locks](https://www.postgresql.org/docs/current/explicit-locking.html)
- [Advisory locks and how to use them](https://shiroyasha.io/advisory-locks-and-how-to-use-them.html)
- [Distributed Locking with Postgres Advisory Locks](https://rclayton.silvrback.com/distributed-locking-with-postgres-advisory-locks)

### Locks — and how to minimize them

**!!!** **Creating indexes and modifying tables requires locks.**

Attempting to take an exclusive lock (and not getting it) will still block concurrent users. Waiting ten seconds for an exclusive lock you hold for 10 milliseconds block others for 10.01 seconds.

- **Must read:** [**PostgreSQL at Scale: Database Schema Changes Without Downtime**](https://medium.com/paypal-tech/postgresql-at-scale-database-schema-changes-without-downtime-20d3749ed680)
- [lock_timeout setting](https://www.postgresql.org/docs/13/runtime-config-client.html#GUC-LOCK-TIMEOUT)
- [CREATE [UNIQUE] INDEX CONCURRENTLY: Adding indexes without blocking writes](https://www.postgresql.org/docs/current/sql-createindex.html#SQL-CREATEINDEX-CONCURRENTLY)
- [Explaining create index concurrently](https://www.2ndquadrant.com/en/blog/create-index-concurrently/) (with bonus Heap Only Tuples-coverage)
- [ALTER TABLE … ADD CONSTRAINT … NOT VALID](https://www.postgresql.org/docs/current/sql-altertable.html#SQL-ALTERTABLE-NOTES): Add constraints with minimal locking
- [Blog post making it more approachable](https://medium.com/doctolib/adding-a-not-null-constraint-on-pg-faster-with-minimal-locking-38b2c00c4d1c) (NOT NULL has become less of a problem since)
- [Why You Should Care That Your SQL DDL is Transactional](https://julien.danjou.info/why-you-should-care-that-your-sql-ddl-is-transactional/)
- [PostgreSQL locking, part 1: Row locks](https://www.percona.com/blog/2018/10/16/postgresql-locking-part-1-row-locks/)
- [PostgreSQL locking, part 2: heavyweight locks](https://www.percona.com/blog/2018/10/24/postgresql-locking-part-2-heavyweight-locks/)

### Deferred constraints

A deferred constraint gets evaluated when attempting to COMMIT, not right away as the row is inserted. Important for e.g. cyclic foreign key constraints, e.g. when you require something in two tables or none of them.

- [Constraints training (Crunchy Data Interactive Learning Portal for Developers)](https://learn.crunchydata.com/postgresql-devel/courses/basics/constraints)
- [Postgres docs on SET CONSTRAINTS](https://www.postgresql.org/docs/current/sql-set-constraints.html)
- [Deferrable SQL Constraints in Depth](https://begriffs.com/posts/2017-08-27-deferrable-sql-constraints.html)

### [Indexes](1.%20Software%20Engineering/3.%20Database/OTLP/SQL/2.%20Indexes/_Base.md) on expressions and partial indexes

### [Range types](GiST%20indexes.md)

```SQL
CREATE TABLE reservation (room int, during tsrange);
INSERT INTO reservation VALUES
    (1108, '[2010-01-01 14:30, 2010-01-01 15:30)');
```

### Exclusion constraints

Typically used with ranges, exclusion constraints can ensure that e.g. the same room doesn’t get booked twice in overlapping time ranges.

- [Postgres docs on exclusion constraints](https://www.postgresql.org/docs/current/ddl-constraints.html#DDL-CONSTRAINTS-EXCLUSION)
- [Exclusion operators: beyond unique](https://www.cybertec-postgresql.com/en/postgresql-exclude-beyond-unique/)

### Webhooks (`pgstream`)

[pgstream](https://xata.io/pgstream) is a CDC ([[Change-Data-Capture]]) tool focused on PostgreSQL. Among other [things](https://github.com/xataio/pgstream?tab=readme-ov-file#features), it can be used to call webhooks whenever there is a data (or schema) change in a Postgres database. This means that whenever a row is inserted, updated, or deleted, or a table is created, altered, truncated or deleted, a webhook is notified of the relevant event details.

![](Pasted%20image%2020241009011327.png)

- [Postgres webhooks with pgstream](https://xata.io/blog/postgres-webhooks-with-pgstream?ref=blog.vvsevolodovich.dev)

### LISTEN / NOTIFY

> Notify payloads need to be small and are only delivered to live connections. Not a queuing solution in their own!

Use cases could be e.g. waking up a syncing process to quickly sync things into Elasticsearch.

- [Postgres docs on LISTEN/NOTIFY](https://www.postgresql.org/docs/current/libpq-notify.html)
- [PostgreSQL LISTEN/NOTIFY blog post](https://tapoueh.org/blog/2018/07/postgresql-listen-notify/)

### Lateral joins

A lateral join is a bit like “for each”, which lets you execute a sub-query per row (which can refer to that row).

Lateral joins combined with JSON aggregates are the foundation for GraphQL-engines building on Postgres.

- [JSON-training (Crunchy Data Interactive Learning Portal for Developers)](https://learn.crunchydata.com/postgresql-devel/courses/beyond-basics/qjsonintro/) : JSON
- [Lateral join training (Crunchy Data Interactive Learning Portal for Developers)](https://learn.crunchydata.com/postgresql-devel/courses/beyond-basics/lateral/)
- [Building a GraphQL to SQL Compiler on Postgres, MS SQL and MySQL](https://hasura.io/blog/building-a-graphql-to-sql-compiler-on-postgres-ms-sql-and-mysql/)

### Window functions

Window functions can do aggregates and other functions (such as ranking) across “windows” without strictly grouping rows. A window can be all rows sharing a value, _n_ rows before or after the “current” row, etc. (Window functions are computed in a final pass over the result set, whereas groupings change the result set)

```SQL
SELECT depname, empno, salary, avg(salary) OVER (PARTITION BY depname) FROM empsalary;
```

- [Postgres tutorial on window functions](https://www.postgresql.org/docs/current/tutorial-window.html)
- [Postgres Window Magic](https://momjian.us/main/writings/pgsql/window.pdf)

### What’s up in my database?

- [pg_stat_activity](https://www.postgresql.org/docs/current/monitoring-stats.html#MONITORING-PG-STAT-ACTIVITY-VIEW): view with one row per server process, showing what that process is up to (or waiting for)
- [Deep dive into postgres stats: pg_stat_activity and pg_locks](https://dataegret.com/2017/10/deep-dive-into-postgres-stats-pg_stat_activity-and-pg_locks/)
- [**PostgresDBA: useful canned queries for your .psqlrc**](https://github.com/NikolayS/postgres_dba)
- [application_name](https://www.postgresql.org/docs/13/runtime-config-logging.html#GUC-APPLICATION-NAME): Name your client, to make it easier to spot in logs and in pg_stat_activity
- [Getting the Most of out Application_Name](https://www.enterprisedb.com/blog/getting-most-out-applicationname)
- [pg_stat_statements](https://www.postgresql.org/docs/current/pgstatstatements.html): tracking planning and execution statistics of all SQL statements executed by a server
- [The most useful Postgres extension: pg_stat_statements](https://www.citusdata.com/blog/2019/02/08/the-most-useful-postgres-extension-pg-stat-statements/)

# References:

1. ~~[Postgres can do THAT?](https://medium.com/cognite/postgres-can-do-that-f221a8046e)~~
2. ! [The Internals of PostgreSQL](https://www.interdb.jp/pg/)
3. [Postgres Architecture Explained](https://www.youtube.com/watch?v=Q56kljmIN14&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=104) (video)
4. [Postgres от начала веков и до наших дней](https://www.youtube.com/watch?v=LlIEboRi4m8) (video)
5. [PostgreSQL, что в логе твоем?](https://habr.com/ru/companies/tensor/articles/696804/)
6. [Postgres Superpowers in Practice](https://event-driven.io/en/postgres_superpowers/)
7. [Как не «проспать» проблемы в базах данных Postgres](https://www.youtube.com/watch?v=MLS6L0QaiC4) (video)
8. [Типичные ошибки при разработке приложений, работающих с PostgreSQL](https://www.youtube.com/watch?v=dDryrO8y82c&list=PLH-XmS0lSi_zgalbXwsytGNdAlNYmmE5C&index=27) (video)
9. [Топ ошибок со стороны разработки при работе с PostgreSQL](https://www.youtube.com/watch?v=HjLnY0aPQZo&list=PLH-XmS0lSi_wMtn1TsBc2_vv7tBDAf7Qg&index=9) (video)
10. [Типовые ошибки в приложениях, которые ведут к bloat в postgresql](https://www.youtube.com/watch?v=-GNHIHEHDmQ&list=PLH-XmS0lSi_xHPmiMdgH9uSW9vBK1yP1A&index=9) (video)
11. [PostgreSQL Antipatterns: насколько глубока кроличья нора? пробежимся по иерархии](https://habr.com/ru/company/tensor/blog/501614/)
12. [Девять способов выстрелить себе в ногу с PostgreSQL](https://habr.com/ru/articles/731942/)
13. [PostgreSQL Antipatterns: простой(?) INSERT… VALUES](https://habr.com/ru/companies/tensor/articles/702902/)
14. [Наполняем до краев: влияние порядка столбцов в таблицах на размеры баз данных PostgresQL](https://habr.com/ru/articles/756074/)

## Logical Replication

1. [Логическая репликация и уровни изоляции транзакций PostgreSQL.](https://www.youtube.com/watch?v=5i07k-uvxXY) (video)
2. [Логическая репликация и Avito](https://www.youtube.com/watch?v=vCYGOVa3w1g) (video)
3. [Evteev & Tyurin: Recovery use cases for Logical Replication in PostgreSQL 10](https://www.youtube.com/watch?v=kk_jwyQwyyk) (video)
4. [Recovery use cases for Logical Replication in PostgreSQL 10](https://medium.com/avitotech/recovery-use-cases-for-logical-replication-in-postgresql-10-a1e6bab03072)

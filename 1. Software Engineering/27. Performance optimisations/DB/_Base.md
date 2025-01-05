!!! [What Every Developer Should Know About SQL Performance](https://use-the-index-luke.com/sql/table-of-contents)

!!! [SQL tuning](https://github.com/donnemartin/system-design-primer#sql-tuning) 

SQL tuning is a broad topic and many [books](https://www.amazon.com/s/ref=nb_sb_noss_2?url=search-alias%3Daps&field-keywords=sql+tuning) have been written as reference.

It's important to **benchmark** and **profile** to simulate and uncover bottlenecks.
- **Benchmark** - Simulate high-load situations with tools such as [ab](http://httpd.apache.org/docs/2.2/programs/ab.html).
- **Profile** - Enable tools such as the [slow query log](http://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) to help track performance issues.

Benchmarking and profiling might point you to the following optimizations.

1. [MySQL 8 Observability и анализ запросов с помощью PMM](https://www.youtube.com/watch?v=LpT-RCMvz88&list=PLH-XmS0lSi_xQtVkWsUMSVUScK_3G_LUP&index=3) (video)
2. [Нормализация SQL profiler трейса для группировки](https://habr.com/ru/articles/647449/)
3. [Explaining the Postgres Query Optimizer](https://momjian.us/main/writings/pgsql/optimizer.pdf)
4. [Explain Explained in PostgreSQL - How Databases Prepare Optimal Query Plans to Execute SQL](https://www.youtube.com/watch?v=P7EUFtjeAmI&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=9) (video)

# How to investigate

> `BUFFERS` extends `EXPLAIN` by adding values to describe the data read/written by each operation => *We’re not only getting info on the query plan, but also on the actual data we move around*

1. Always use `BUFFERS` when running an `EXPLAIN`. It gives some data that may be crucial for the investigation. `Buffers: shared hit=1166 read=3117` => **1166 blocks (9,551,872 bytes) were provided by the shared cache**, but **3117 blocks (25,534,464 bytes) had to be read from disk**. => we can take a look at the places where we read a lot of data from disk and try to optimise them. 
2. Always, always try to get an `Index Cond` (called `Index range scan` in MySQL) instead of a `Filter`.
3. Always, always, always assume PostgreSQL and MySQL will behave differently. Because they do.

# Best Practices:
## 1. [[Tighten up the schema]]

## 2. Use good [Indexes](../../../../3.%20Database/OTLP/SQL/2.%20Indexes/_Base.md)

- Columns that you are querying (`SELECT`, `GROUP BY`, `ORDER BY`, `JOIN`) could be faster with indices.
- Indices are usually represented as self-balancing [B-tree](https://en.wikipedia.org/wiki/B-tree) that keeps data sorted and allows searches, sequential access, insertions, and deletions in logarithmic time.
- Placing an index can keep the data in memory, requiring more space.
- Writes could also be slower since the index also needs to be updated.
- **When loading large amounts of data, it might be faster to disable indices, load the data, then rebuild the indices.**
- Avoid creating too many indexes to prevent insert and update performance impact and storage consumption.

[Sometimes indexes can be not effective](1.%20Software%20Engineering/3.%20Database/OTLP/SQL/2.%20Indexes/_Base.md) (`SHOWPLAN_TEXT` to check):

## 3. Avoid expensive joins

- [Denormalize](https://github.com/donnemartin/system-design-primer#denormalization) where performance demands it.
- Join small tables first to reduce dataset size before joining larger tables, improving query performance.
- Avoid excessive table joins to prevent performance degradation and complexity in queries.
- Be cautious of join types and conditions to avoid unintended Cartesian products or inefficient query plans.
## 4. [Partitioning](../../../../3.%20Database/OTLP/SQL/5.%20Distributed/Partitioning/Partitioning.md) tables

- Break up a table by putting hot spots in a separate table to help keep it in memory.
## 5. Tune the query cache

- In some cases, the [query cache](https://dev.mysql.com/doc/refman/5.7/en/query-cache.html) could lead to [performance issues](https://www.percona.com/blog/2016/10/12/mysql-5-7-performance-tuning-immediately-after-installation/).
## 6. [Sharding](../../../../3.%20Database/OTLP/SQL/5.%20Distributed/Sharding/Sharding.md) DB

## 7. Configuration improvements

1. [Филипп Дельгядо — СУБД: индивидуальный пошив и подгонка по фигуре](https://www.youtube.com/watch?v=l4l5pLlC40U) (video)
2. [Tuning Your PostgreSQL Server](https://wiki.postgresql.org/wiki/Tuning_Your_PostgreSQL_Server)
3. [Performance Tuning PostgreSQL](https://www.revsys.com/writings/postgresql-performance.html)

## 8. [Tune queries](https://github.com/donnemartin/system-design-primer#sql-tuning)

- Prefer `UNION ALL` over `UNION` for combining result sets to avoid removing duplicate rows and improve query efficiency.
- Execute multiple operations in a single batch to minimize round trips between the application and database server.
- [Use OFFSET and FETCH NEXT for efficient pagination instead of LIMIT.](Offsets.md)
- Rewrite subqueries as join operations for better performance.
- Optimize GROUP BY queries by including only necessary columns and applying appropriate aggregate functions.
- [Improve INSERT Performance](Improve%20INSERT%20Performance.md)

## 9. Maybe add abstraction on the top

Youtube add [an abstraction layer on top of MySQL for simplicity and scalability.](https://newsletter.systemdesign.one/p/vitess-mysql?utm_source=substack&publication_id=1511845&post_id=144987552&utm_medium=email&utm_content=share&utm_campaign=email-share&triggerShare=true&isFreemail=true&r=1vxw4z&triedRedirect=true)

**Interacting with DB**: They installed a [sidecar server](https://learn.microsoft.com/en-us/azure/architecture/patterns/sidecar) in front of each MySQL instance and called it **VTTablet**.
![](Pasted%20image%2020240609181735.png)

It let them:
- Control MySQL server and manage database backups
- Rewrite expensive queries by adding the limit clause
- Cache frequently accessed data to prevent the [thundering herd problem](https://en.wikipedia.org/wiki/Thundering_herd_problem)

**Routing SQL Queries:** They set up a stateless proxy server to route the queries and called it **VTGate**.
![](Pasted%20image%2020240609181820.png)

It let them:
- Find the correct VTTablet to route a query based on the schema and sharding scheme
- Keep the number of MySQL connections low via [connection pooling](https://www.prisma.io/dataguide/database-tools/connection-pooling#:~:text=Broadly%20speaking%2C%20connection%20pooling%20refers,an%20external%20tool%20or%20service.)
- Speak MySQL protocol with the application layer
- Act like a monolithic MySQL server for simplicity
- Limit the number of transactions at a time for performance

**State Information:** They set up a distributed **key-value database** ([[Zookeeper]]) to store information about schemas, sharding schemes, and roles
![](Pasted%20image%2020240609181950.png)

Also it takes care of relationships between databases like the leader and followers.

Besides they cache this data on VTGate for better performance.

They run an HTTP server to keep the key-value database updated. And called it **VTctld**: it gets the entire list of servers and their relationships and then updates the key-value database.

![](Pasted%20image%2020240609182250.png)

- VTGate: proxy server to route queries
- Key-Value Database: configuration server for topology management
- VTTablet: sidecar server running on each MySQL

# References:

1. ~~[Best Practices Working with Billion-row Tables in Databases](https://www.youtube.com/watch?v=wj7KEMEkMUE&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=48) (video)~~
2. ~~[Buffer analysis when using EXPLAIN ANALYSE in Postgres](https://willj.net/posts/buffer-analysis-when-using-explain-analyse-in-postgres/)~~
3. ~~[Making a Postgres query 1,000 times faster](https://mattermost.com/blog/making-a-postgres-query-1000-times-faster/?utm_source=substack&utm_medium=email)~~
4. ~~[How YouTube Was Able to Support 2.49 Billion Users With MySQL](https://newsletter.systemdesign.one/p/vitess-mysql?utm_source=substack&publication_id=1511845&post_id=144987552&utm_medium=email&utm_content=share&utm_campaign=email-share&triggerShare=true&isFreemail=true&r=1vxw4z&triedRedirect=true)~~
5. [11 Database Optimization Techniques](https://danielfoo.medium.com/11-database-optimization-techniques-97fdbed1b627)
6. [8 Techniques To Speed up Your Database](https://betterprogramming.pub/8-techniques-to-speed-up-your-database-292754ff7739)
7. [Three things you should never put in your database](https://www.revsys.com/tidbits/three-things-you-should-never-put-your-database/)
8. [Поиск проблем в базе данных, если ты разработчик / Алексей Лесовский (Coins.ph)](https://www.youtube.com/watch?v=h8UIX94XJGc) (video)
9. [**Understanding Postgres Performance**](https://www.craigkerstiens.com/2012/10/01/understanding-postgres-performance/)
10. [**More on Postgres Performance**](https://www.craigkerstiens.com/2013/01/10/more-on-postgres-performance/)
11. **[PostgreSQL 13. Оптимизация запросов](https://postgrespro.ru/education/courses/QPT)** (course)
12. [SQL Optimization Strategies for Performance Improvement](https://jinlow.medium.com/sql-optimization-strategies-for-performance-improvement-bd8138fcbcc5)
13. [11 SQL Query Optimization Techniques commonly used in Projects](https://experiencestack.co/11-sql-query-optimization-techniques-commonly-used-in-projects-ed45c31c45cd)
14. [SQL Optimization Strategies for Performance Improvement](https://jinlow.medium.com/sql-optimization-strategies-for-performance-improvement-bd8138fcbcc5)
15. [Top Performance issues every developer/architect must know — part 1-Database](https://medium.com/javarevisited/top-performance-issues-every-developer-architect-must-know-part-1-fc1ad6e1644b)
16. [Почему PostgreSQL тормозит: индексы и корреляция данных](https://habr.com/ru/company/ozontech/blog/564520/)
17. [Анализ запросов в MySQL, PostgreSQL, MongoDB](https://www.youtube.com/watch?v=dJR10fEH6uM&list=PLH-XmS0lSi_x0OrxrC4GKInFRK8zG_tfZ&index=9) (video)
18. [Длинная транзакция или когда размер имеет значение](https://www.youtube.com/watch?v=3h48iowNbwo) (video)
19. [Дорогой DELETE](https://www.youtube.com/watch?v=fVF1PoKplps&list=PLH-XmS0lSi_zTZrols83QSxI3Q96dSbBm&index=35) (video)
20. [One Database Transaction Too Many](https://hakibenita.com/django-nested-transaction)
21. [Необычные случаи оптимизации производительности на примере ClickHouse](https://www.youtube.com/watch?v=GW07RZVpH4M&list=PLH-XmS0lSi_xQtVkWsUMSVUScK_3G_LUP&index=19) (video)
22. [Отъявленные баги и как их избежать на примере ClickHouse](https://www.youtube.com/watch?v=ooBAQIe0KlQ&list=PLH-XmS0lSi_zTZrols83QSxI3Q96dSbBm&index=93) (video)
23. [Не изобретая велосипед. Кэширование: рассказываем главные секреты оптимизации доступа к данным](https://habr.com/ru/company/stm_labs/blog/654201/)
24. [30x Performance Improvements on MySQLStreamer at Yelp](https://engineeringblog.yelp.com/2018/02/making-30x-performance-improvements-on-yelps-mysqlstreamer.html)
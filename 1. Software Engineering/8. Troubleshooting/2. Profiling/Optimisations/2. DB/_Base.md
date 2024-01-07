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

# Best Practices:
## 1. [[Tighten up the schema]]

## 2. Use good [Indexes](../../../../3.%20Database/OTLP/2.%20Indexes/_Base.md)

- Columns that you are querying (`SELECT`, `GROUP BY`, `ORDER BY`, `JOIN`) could be faster with indices.
- Indices are usually represented as self-balancing [B-tree](https://en.wikipedia.org/wiki/B-tree) that keeps data sorted and allows searches, sequential access, insertions, and deletions in logarithmic time.
- Placing an index can keep the data in memory, requiring more space.
- Writes could also be slower since the index also needs to be updated.
- **When loading large amounts of data, it might be faster to disable indices, load the data, then rebuild the indices.**
## 3. Avoid expensive joins

- [Denormalize](https://github.com/donnemartin/system-design-primer#denormalization) where performance demands it.
## 4. [Partitioning](../../../../3.%20Database/OTLP/5.%20Distributed/Partitioning/Partitioning.md) tables

- Break up a table by putting hot spots in a separate table to help keep it in memory.
## 5. Tune the query cache

- In some cases, the [query cache](https://dev.mysql.com/doc/refman/5.7/en/query-cache.html) could lead to [performance issues](https://www.percona.com/blog/2016/10/12/mysql-5-7-performance-tuning-immediately-after-installation/).
## 6. [Sharding](../../../../3.%20Database/OTLP/5.%20Distributed/Sharding.md) DB

## 7. Configuration improvements

1. [Филипп Дельгядо — СУБД: индивидуальный пошив и подгонка по фигуре](https://www.youtube.com/watch?v=l4l5pLlC40U) (video)
2. [Tuning Your PostgreSQL Server](https://wiki.postgresql.org/wiki/Tuning_Your_PostgreSQL_Server)
3. [Performance Tuning PostgreSQL](https://www.revsys.com/writings/postgresql-performance.html)

# References:

1. ~~[Best Practices Working with Billion-row Tables in Databases](https://www.youtube.com/watch?v=wj7KEMEkMUE&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=48) (video)~~
2. [11 Database Optimization Techniques](https://danielfoo.medium.com/11-database-optimization-techniques-97fdbed1b627)
3. [8 Techniques To Speed up Your Database](https://betterprogramming.pub/8-techniques-to-speed-up-your-database-292754ff7739)
4. [Three things you should never put in your database](https://www.revsys.com/tidbits/three-things-you-should-never-put-your-database/)
5. [Поиск проблем в базе данных, если ты разработчик / Алексей Лесовский (Coins.ph)](https://www.youtube.com/watch?v=h8UIX94XJGc) (video)
6. [**Understanding Postgres Performance**](https://www.craigkerstiens.com/2012/10/01/understanding-postgres-performance/)
7. [**More on Postgres Performance**](https://www.craigkerstiens.com/2013/01/10/more-on-postgres-performance/)
8. **[PostgreSQL 13. Оптимизация запросов](https://postgrespro.ru/education/courses/QPT)** (course)
9. [SQL Optimization Strategies for Performance Improvement](https://jinlow.medium.com/sql-optimization-strategies-for-performance-improvement-bd8138fcbcc5)
10. [11 SQL Query Optimization Techniques commonly used in Projects](https://experiencestack.co/11-sql-query-optimization-techniques-commonly-used-in-projects-ed45c31c45cd)
11. [SQL Optimization Strategies for Performance Improvement](https://jinlow.medium.com/sql-optimization-strategies-for-performance-improvement-bd8138fcbcc5)
12. [Top Performance issues every developer/architect must know — part 1-Database](https://medium.com/javarevisited/top-performance-issues-every-developer-architect-must-know-part-1-fc1ad6e1644b)
13. [Почему PostgreSQL тормозит: индексы и корреляция данных](https://habr.com/ru/company/ozontech/blog/564520/)
14. [Анализ запросов в MySQL, PostgreSQL, MongoDB](https://www.youtube.com/watch?v=dJR10fEH6uM&list=PLH-XmS0lSi_x0OrxrC4GKInFRK8zG_tfZ&index=9) (video)
15. [Длинная транзакция или когда размер имеет значение](https://www.youtube.com/watch?v=3h48iowNbwo) (video)
16. [Дорогой DELETE](https://www.youtube.com/watch?v=fVF1PoKplps&list=PLH-XmS0lSi_zTZrols83QSxI3Q96dSbBm&index=35) (video)
17. [One Database Transaction Too Many](https://hakibenita.com/django-nested-transaction)
18. [Необычные случаи оптимизации производительности на примере ClickHouse](https://www.youtube.com/watch?v=GW07RZVpH4M&list=PLH-XmS0lSi_xQtVkWsUMSVUScK_3G_LUP&index=19) (video)
19. [Отъявленные баги и как их избежать на примере ClickHouse](https://www.youtube.com/watch?v=ooBAQIe0KlQ&list=PLH-XmS0lSi_zTZrols83QSxI3Q96dSbBm&index=93) (video)
20. [Не изобретая велосипед. Кэширование: рассказываем главные секреты оптимизации доступа к данным](https://habr.com/ru/company/stm_labs/blog/654201/)
21. [30x Performance Improvements on MySQLStreamer at Yelp](https://engineeringblog.yelp.com/2018/02/making-30x-performance-improvements-on-yelps-mysqlstreamer.html)
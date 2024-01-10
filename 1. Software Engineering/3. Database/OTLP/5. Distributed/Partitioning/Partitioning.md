## [Partitioning vs Sharding](../Partitioning%20vs%20Sharding.md)

## Pros of Partitioning

(+) Improves query performance when accessing a single partition
	This is especially important for very large tables with big indexes that can not be fitted into memory
(+) Sequential scan vs scattered index scan
(+) Easy bulk loading (attach partition)
	 Example: create tables from CSV files on the fly
(+) Archive old data that are barely accessed into cheap storage
## Cons of Partitioning

(-) Updates that move rows from a partition to another (slow or fail sometimes)
	Can harm SSD disks
(-) Inefficient queries could accidentally scan all partitions resulting in slow performance
(-) Schema changes can be challenging. 
	DB has to support this feature, but sometimes fails.
## [Partitioning Strategies](Partitioning%20Strategies.md):

![Pasted image 20230605121902](../../../../../_Attachments/Pasted%20image%2020230605121902.png)
1. **Horizontal Partitioning** splits rows into partitions
	range or list
2. **Vertical Partitioning** splits columns into partitions
	large column (blob) that you can store in a slow access drive in its own table space

*More modern databases like Cassandra and others abstract that away from the application logic and its maintained at the database level.*
## [Partitioning Types](Partitioning%20Types.md)

## Demo with Postgres

Example:

We have a table `grades_org` with `id` and a random grade (`g`)
```SQL
create table grades_org (id serial not null, g int not null);

insert into grades_org(g) select floor(random()*100) from generate_series(0, 10000000);
```

![Pasted image 20231217171635](../../../../../_Attachments/Pasted%20image%2020231217171635.png)

1. Create main partition table to which clients will refer:
   `create table grades_parts (id serial not null, g int not null) partition by range(g);`
2. Create partition tables for each range:
	1. `create table g0035 (like grades_parts including indexes);`
	2. `create table g3560 (like grades_parts including indexes);`
	3. `create table g6080 (like grades_parts including indexes);`
	4. `create table g80100 (like grades_parts including indexes);`
3. Attach each partition to it's values range:
	1. `alter table grades_parts attach partition g0035 for values from (0) to (35);`
	2. `alter table grades_parts attach partition g3560 for values from (35) to (60);`
	3. `alter table grades_parts attach partition g6080 for values from (60) to (80);`
	4. `alter table grades_parts attach partition g80100 for values from (80) to (100);`
4. Insert data into main partition table: `insert into grades_parts select * from grades_org;`
	All data will be put to corresponding partition table automatically
	![Pasted image 20231217171303](../../../../../_Attachments/Pasted%20image%2020231217171303.png)
5. Create an index for `grades_partits` table. The index will be created auto for all partitions `create index grades_parts_indx on grades_parts(g);`
	![Pasted image 20231217171435](../../../../../_Attachments/Pasted%20image%2020231217171435.png)
6. Querying `explain analyze select count(*) from grades_parts where g = 30;`
	![Pasted image 20231217171522](../../../../../_Attachments/Pasted%20image%2020231217171522.png)


**Table size and index size comparison:**

![Pasted image 20231217155858](../../../../../_Attachments/Pasted%20image%2020231217155858.png)

Make sure `ENABLE_PARTITION_PRUNING = true` 

![Pasted image 20231217160024](../../../../../_Attachments/Pasted%20image%2020231217160024.png)

# References:

1. ~~[Database Sharding Explained](!https://architecturenotes.co/database-sharding-explained/ )~~
2. [Mastering the Art of Data Partitioning for System Design Interviews: A Complete Guide](https://levelup.gitconnected.com/master-the-art-of-data-partitioning-for-system-design-interviews-a-complete-guide-b9d8075bc3cb)
2. [Mastering PostgreSQL Table Partitioning](https://fragland.dev/a-guide-to-table-partitioning-with-postgresql-12)
3. [Bulkheads: Partition and Tolerate Failure in One Part](https://skife.org/architecture/fault-tolerance/2009/12/31/bulkheads.html)
4. ~~[Database Partitioning Crash Course (with Postgres)](https://www.youtube.com/watch?v=sitUYx2EfhY&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=12) (video)~~
5. [Calvin: Fast Distributed Transactions for Partitioned Database Systems](https://ydb.tech/ru/docs/) (paper)
6. [How to Automate Partitioning in Postgres](https://www.youtube.com/watch?v=Ts6lx3sImQo&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=60) (video)
7. [Horizontal vs Vertical Database Partitioning](https://www.youtube.com/watch?v=QA25cMWp9Tk&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=13) (video)
8. [Horizontal Partitioning In System Design | SDE Interview](https://ganeshprasad227.medium.com/horizontal-partitioning-in-system-design-sde-interview-c6894142c438)
9. [Database Partitioning](https://github.com/vitkarpov/coding-interviews-blog-archive/blob/main/posts/database-partitioning.md)
10. [Partitioning Tables](https://www.vertica.com/docs/9.2.x/HTML/Content/Authoring/AdministratorsGuide/Partitions/PartitioningTables.htm)
11. [How We Partitioned Airbnb’s Main Database in Two Weeks](https://medium.com/airbnb-engineering/how-we-partitioned-airbnb-s-main-database-in-two-weeks-55f7e006ff21)
12. [CRDT. Бесконфликтная синхронизация данных](https://youtu.be/j-CFTQVuP-s)


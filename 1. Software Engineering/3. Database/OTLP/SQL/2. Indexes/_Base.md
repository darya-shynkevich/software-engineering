##### ==*Scale of data often works against you, and balanced trees are the first tool in your arsenal against it.*==

A Database Index is Like a Book Index: *When you want to look something up in a book, you don’t have to start at page one and read every page until you find the thing you’re looking for. Instead, you look in the index, which directs you to a page in the book. Then you flip to that page and voila, you find the thing.*

*There is one big difference between book indexes and database indexes: you’re probably updating the contents of the database constantly, while a book is indexed only when a new version is published.*

*So unlike a book, the database needs to keep its index (really, it’s indexes — there are often many) up to date automatically and frequently.*

# [B-tree Index](Types/B-tree%20Index.md)

**[B-tree Index](Types/B-tree%20Index.md)** speeds up searching by maintaining a tree whose [Leaf Nodes](../1.%20Internals/Database%20Pages/Leaf%20Nodes.md) are pointers to the disk locations of table records.

> **Formal definition:** *A database index functions as a specialized structure designed to **accelerate the speed and efficiency of query operations** within a database. Read performance increases as you index the data, but that comes at the cost of write performance since you need to keep index up to date.*

1. supported by all DBMSs for all data types
2. optimal for columns with high selectivity, and if the column is used for both equality checks and range scans
3. a very important aspect of B+Tree indexes is that they are sorted by the index key => auto-incremented column values or monotonically increasing timestamp values make very good candidates for a B+Tree since the index will grow in the same direction, therefore providing a very good *page fill factor*
4. random column values are not very suitable for a B+Tree index because the newer elements will not necessarily fill the existing index nodes as they may require their own nodes due to the randomness of the key values => the B+Tree index would have a low fill factor and a very large size on the disk that would put pressure on the in-memory Buffer Pool
5. since the B+Tree index is already sorted, as long as we are interested in displaying the result set on the ascending or descending order of an index, then the PostgreSQL query optimizer may choose to scan the indexed records in that particular order

# Other Index Types

### 1. [[GIN indexes]] for JSON data in PostgreSQL

**MySQL and SQLite** both have some support for working with JSON data, but neither offers a specialized index type like the GIN index suitable for indexing JSON directly. However, with MySQL you can index a column generated from JSON data

**PostgreSQL** offers two JSON field types: `json` and `jsonb`. Django’s `JSONField` uses the `jsonb` column type, so any references to JSON data in a PostgreSQL database in this section refer to the `jsonb` column type.

GIN stands for **Generalized Inverted Index**. It is an index type that works particularly well with "contains" logic for JSON data and full-text search.

> `properties` is `jsonb` field

```sql
CREATE INDEX idx_properties_gin ON book USING GIN (properties)
```

```sql
SELECT *
FROM table_name
WHERE
    properties ? 'publisher'
```


Another type of SQL query that we can speed up with the GIN index is the one that has to filter by a given attribute name and value combination.

```sql
CREATE INDEX idx_properties_gin ON book USING GIN (properties jsonb_path_ops)
```

With the `idx_properties_gin` index supporting the `jsonb_path_ops` operator, when running the following SQL query that searches if there is an existing `title` attribute with the value of `High-Performance Java Persistence` inside the `properties` JSON column value:

```sql
SELECT title, author, published_on
FROM book
WHERE
  properties @> '{"title": "High-Performance Java Persistence"}'
```

*The SQL query filters the `book` table rows using the `idx_properties_gin` GIN Index, and only `6` pages are scanned from the PostgreSQL Shared Buffers.*
### 2. [GiST indexes](GiST%20indexes.md) for spatial data in PostgreSQL

### 3. [BRIN indexes](BRIN%20indexes) for very large tables in PostgreSQL

### 4. [Hash indexes](Hash%20indexes) for in-memory tables in MySQL

Hash indexes store a 32-bit hash code derived from the value of the indexed column. Hence, such indexes can only handle simple equality comparisons. The query planner will consider using a hash index whenever an indexed column is involved in a comparison using the **equal operator**

Storing not the values ​​themselves, but their hashes.
(+) size reduction (if column = 50 characters => 50 bytes to store index; for hash index it is only 4 bytes). The more compact the indexes, the better chance they would fit into the PostgreSQL Shared Buffers or the OS cache.
(+) comparison acceleration ==

(-) operations >, <, is null, etc. are not possible
(-) collision problem

```sql
CREATE INDEX IF NOT EXISTS idx_column_name_hash ON table_name USING HASH (column_name)
```
#### References:

1. [The Power of Database Indexing Algorithms: B-Tree vs. Hash Indexing](https://dip-mazumder.medium.com/the-power-of-database-indexing-algorithms-b-tree-vs-hash-indexing-6e3a4112a81)

### 5. [R-tree indexes](Types/R-tree%20indexes.md) for spatial data in MySQL

PostgreSQL: point, line, polygon, box, path, circle
MySQL: geometry, point, polygon, ...
### 6. Covering Indexes ([Index Only Scan](Index%20Scan%20vs%20Index%20Only%20Scan.md), INCLUDE)

An index is said to "cover" a query if the database can return all of the data needed for the query directly from the index, without having to search through table data.

For example, if you search for "What is a covering index?", Google presents an answer pulled directly from Stack Overflow, ***so that you don’t have to click the link and visit the site.*** 

A database can do the same thing if your query only references columns that are stored in the index. Let’s take a look at an example using Postgres. 

![Pasted image 20231201001549](../../../../../_Attachments/Pasted%20image%2020231201001549.png)

1. Create multi-column index on (name, id)
2. Now we get an Index Only Scan.

> The order of columns given to `CREATE INDEX` matters. That is, an index on `(id, name)` is not the same as an index on `(name, id)` . In the case of our query, PostgreSQL would not even use an index on `(id, name)` because a simple table scan would be faster. This is because trying to use an index on `(id, name)` for a query that filters on name but not id would involve scanning the entire index. However, an index on `(name, id)` can be used because the index is organized around name and name is used in a WHERE clause. Dwell on these mysteries in silence for a moment.

We can do it better with Postgres using `INCLUDE`:
![Pasted image 20231201002047](../../../../../_Attachments/Pasted%20image%2020231201002047.png)
The `INCLUDE` version of the index is 21 milliseconds faster, so in this case, the difference is not huge. Still, it’s an optimization worth exploring with your data and queries.

It's wise to be conservative about adding non-key columns to an index, especially wide columns. ***If an index tuple exceeds the maximum size allowed for the index type, data insertion will fail.*** In any case, non-key columns duplicate data from the index's table and bloat the size of the index, thus potentially ***slowing searches***. Furthermore, B-tree deduplication is never used with indexes that have a non-key column.

***This type of index is useful when queries require specific non-key columns to be retrieved efficiently, without the need to access the table itself.***
### 7. Multiple column index

A multiple column index is created by specifying more than one column in the index definition. It allows for efficient retrieval of data ***based on the combination of values in multiple columns***. The index stores the values of all the indexed columns in a specific order, allowing for quick access to matching rows. This type of index is useful when queries involve equality or range conditions on multiple columns.
### 8. [Partial Indexes](Partial%20Indexes)

A partial index is an index with a `WHERE` clause that limits which rows in the table get added to the index.

Partial indexes can speed up queries on specific and repeatedly used subsets of data in a table, such as rows with a "created" time between two timestamps, or a specific column value like "user.viewed".

![Pasted image 20231201002824](../../../../../_Attachments/Pasted%20image%2020231201002824.png)

### 9. [Clustering](Clustering)

If the table changes infrequently and most of your queries filter on the same set of columns, you have another option: clustering.

Clustering gives a speed boost to queries that return multiple rows in some order (e.g., the order specified by an index). Whereas these rows might occur near each other in an index, they might not be stored near each other on disk, so a query that can find their page locations in the index may still have to search around in different places on disk to find data for the query.

***Clustering fixes this by restructuring the table data on disk so that rows that are organised together in the index are also organised together on disk***

For example, if we have an index on the name field in an analytics_event table called `analytics_event_name_idx` , we could reorganize the table data by writing `CLUSTER analytics_event USING analytics_event_name_idx`.

After clustering, a query for a value of `name` should be faster without us needing to create a partial index just for that value, as shown in the next example.

![Pasted image 20231201003206](../../../../../_Attachments/Pasted%20image%2020231201003206.png)

***Clustering is a one-time operation***. PostgreSQL does not maintain the structure of the table, so subsequent writes may place data in non-contiguous physical locations on disk. Setting the table’s `FILLFACTOR` setting to below 100% will allow updates to stay on the same page if space is left, but sooner or later, you will need to `CLUSTER` again.

> `CLUSTER` requires an `ACCESS EXCLUSIVE` lock on the table, which blocks reads and writes. This could be a problem if you have a frequently-used table and need to CLUSTER it often to maintain your desired physical ordering.

#### Clustered indexes in MySQL

With the InnoD
B engine for MySQL, tables are automatically clustered by their primary key , and the feature is called "clustered indexes."

The InnoDB implementation can speed up range queries based on primary key, and as a bonus, the clustered order is maintained automatically (no need to run a command like `CLUSTER` again, as with Posgres). However, tables cannot be clustered by anything but their primary key. In MySQL parlance, that means you can’t cluster a table by a secondary index.

### 10.  BitMap in PostgreSQL

Создание битовых карт (0101001) для каждого возможного значения столбца.

(+) на больших множествах с низкой мощностью и хорошей кластеризацией по их значению индекс будет меньше чем B-tree
(+) эффективны для AND, OR, XOR запросов
(-) не подходит для столбцов с высокой селективностью (типа `id`)
### 11. Reverse index in Oracle

1. B-tree с реверсивным ключом
2. используется для монотонно возрастающих значений
3. используется в случае когда наименее значимая цифра изменяется чаще, чем наиболее значимая => индекс будет стоится быстрее, тк экономим время на балансировке
### 12. Inverted index

Полнотекстовый индекс, хранящий для каждой лексемы ключей отсортированный список адресов записей таблицы, которые содержат данный ключ
#### References:

1. [So what is an inverted index and how does it work?](https://iorilan.medium.com/so-what-is-an-inverted-index-and-how-does-it-work-95334d9a306d)

### 13. Functional-based index

`HASH` для Oracle

Indexes on `JSONB` in PostgreSQL

PostgreSQL: indexes can be made on expressions which may call functions (e.g `lower(username)` in a unique index to ensure unique lowercase usernames) and partial (e.g. `unique index on assets(project_id, external_id) where deleted_at is not null`) or both.

- [Efficient Use of PostgreSQL Indexes](https://devcenter.heroku.com/articles/postgresql-indexes)
- [Postgres docs on indexing expressions](https://www.postgresql.org/docs/current/indexes-expressional.html)
- [Postgres docs on partial indexes](https://www.postgresql.org/docs/current/indexes-partial.html)

# When the Database Ignores an Index

With Postgres, which is representative of other relational databases in this respect, the query planner/ optimiser may decide not to use an index if: 
1. Table statistics are out of date 
2. The query returned too many results 
3. The table has too many duplicate values 
4. Something exotic

*The decision of the query planner to use an index can come down to the table schema, the data in the table, current statistics, and exactly how the database’s query planner is implemented.*
#### Cost prediction

A simplified version of query planner logic goes something like this: 
1. Build "plans" for the possible ways to execute the query 
2. Predict the "cost" of each plan 
3. Select the plan with the lowest predicted cost 

What is this "cost" that the database tries to predict? It is usually an abstract number representing the disk access and CPU work that the database expects a plan will require.
### 1. Outdated statistics => force update

Most databases implement an `ANALYZE` keyword that will force collecting statistics for a table. The PostgreSQL keyword is `ANALYZE`, while MySQL and SQLite use `ANALYZE TABLE` .
### 2. Too Many Results

As mentioned at the beginning of this section, the query planner won’t use an index if it anticipates returning too many results. This is because B-tree indexes in particular perform best when locating a fraction of the total data in a table => pagination.
### 3. Query Doesn’t Match the Index

Your query may not match the index. For example, an index on the name column won’t match a query that uses both name and age in its `WHERE` clause. Be sure that if you use a field in a `WHERE` clause, you include that field in the index.
	1. **does not satisfy the leftmost prefix rule** — indexes may fail if the query does not use the leftmost column(s) of the index.
	2. **range index columns not placed last** — when creating composite indexes, ensure that columns involved in range operations are placed after columns involved in equality operations.
	3. **using SELECT** -  retrieving all columns using SELECT * may cause index failure as it does not optimize index usage.
	4. **index columns involved in computations** — indexes may fail if the indexed columns are involved in computations or expressions in the query.
	5. **using functions on indexed columns** — applying functions directly to indexed columns in the query can prevent the use of indexes.
	6. **using wildcards (%) on the left side of LIKE** — Using wildcards (%) on the left side of the LIKE operator can prevent index usage.
	7. **using OR operator without caution** — Using the OR operator without caution may lead to index failure as it may prevent the use of indexes efficiently.

# [LSM Trees and SSTables](../1.%20Internals/LSM%20Trees%20and%20SSTables.md)


# References:

1. ! [Build your own Database Index](https://dx13.co.uk/articles/2023/12/02/byo-index-pt1/)
2. [Организация таблиц и индексов](https://studfile.net/preview/4419928/page:4/)
3. [Как устроено индексирование баз данных](https://habr.com/ru/companies/ruvds/articles/724066/)
4. [An in-depth look at Database Indexing](https://medium.com/free-code-camp/database-indexing-at-a-glance-bb50809d48bd)
5. [Database Indexing under the hood](https://towardsdev.com/database-indexing-under-the-hood-5841ac77f7de)
6. ~~[what is a database index?](https://www.youtube.com/watch?v=Jemuod4wKWo&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=96)~~
7. [Why does Innodb Engine use the B+ tree data structure for indexing?](https://iorilan.medium.com/why-does-innodb-engine-use-the-b-tree-data-structure-for-indexing-6c4a41f6f356)
8. [Revisiting B+-tree vs. LSM-tree](https://www.usenix.org/publications/loginonline/revisit-b-tree-vs-lsm-tree-upon-arrival-modern-storage-hardware-built)
9. [B-tree vs B+ tree in Database Systems](https://www.youtube.com/watch?v=UzHl2VzyZS4&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=77) (video)
10. [B-Tree vs B+Tree in Production Database Systems](https://www.youtube.com/watch?v=a1Z40OC553Y&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=2) (video)
11. [B+Tree index structures in InnoDB](https://blog.jcole.us/2013/01/10/btree-index-structures-in-innodb/)
12. [Understanding database Indexes in PostgreSQL](https://mastermind.dev/indexes-in-postgresql)
13. [Индексы в PostgreSQL — 1](https://habr.com/ru/company/postgrespro/blog/326096/)
14. [Database Indexing Explained (with PostgreSQL)](https://www.youtube.com/watch?v=-qNSXK7s7_w&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=7) (video)
15. [Indexing in PostgreSQL vs MySQL](https://www.youtube.com/watch?v=T9n_-_oLrbM&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=8) (video)
16. [Index Fill Factor | The Backend Engineering Show](https://www.youtube.com/watch?v=qXDhMJCuDEc&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=97) (video)
17. [Partial Indexing | The Backend Engineering Show](https://www.youtube.com/watch?v=WL2NXQmUOC0&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=81) (video)
18. [Bitmap Index Scan in Postgres Explained with Examples](https://www.youtube.com/watch?v=rJ-oG0y1hqE&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=57) (video)
19. [Combining Database Indexes for Better Performance in Your Backend Applications](https://www.youtube.com/watch?v=mBonHTpQO7E&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=54) (video)
20. [Explaining Key vs Non-Key Column Database Indexing and How it can Improve Performance](https://www.youtube.com/watch?v=9v3GqKtbqzQ&list=PLQnljOFTspQUybacGRk1b_p13dgI-SmcZ&index=16) (video)
21. [Index Scan vs Index Only Scan on Database Systems (with Postgres)](https://www.youtube.com/watch?v=xsPBT5gIQac&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=46) (video)
22. [Key vs Non-Key Column Database Indexing](https://www.youtube.com/watch?v=Fac-gzudFIA&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=3) (video)
23. [nbtree-индексы в PostgreSQL. Полезные новинки](https://www.youtube.com/watch?v=ueERqeB1YOM&list=PLH-XmS0lSi_xQkDOdfAYsdxjuadMPTw8H&index=2) (video)
24. ~~[PostgreSQL Index Types](https://vladmihalcea.com/postgresql-index-types/)~~







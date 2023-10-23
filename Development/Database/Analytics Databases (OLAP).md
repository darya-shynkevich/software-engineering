These queries are often written by business analysts, and feed into reports that help the management of a company make better decisions (business intelligence). In order to differentiate this pattern of using databases from transaction processing, it has been called online analytic processing (OLAP). The difference between OLTP and OLAP is not always clear-cut, but some typical characteristics are listed in
![[Pasted image 20230604124305.png]]
The OLTP systems are usually expected to be highly available and to process transactions with low latency, since they are often critical to the operation of the business. 

*A data warehouse, by contrast, is a separate database that analysts can query, without affecting OLTP operations.* 

The **data warehouse** contains a read-only copy of the data in all the various OLTP systems in the company. Data is extracted from OLTP databases (using either a periodic data dump or a continuous stream of updates), transformed into an analysis-friendly schema, cleaned up, and then loaded into the data warehouse. This process of getting data into the warehouse is known as ***Extract–Transform–Load (ETL)***

![[Pasted image 20230604124518.png]]

#### **Stars and Snowflakes: Schemas for Analytics**

The name ***“star schema”*** comes from the fact that when the table relationships are visualized, the fact table is in the middle, surrounded by its dimension tables; the connections to these tables are like the rays of a star.
![[Pasted image 20230604124825.png]]

A variation of this template is known as the ***snowflake schema***, where dimensions are further broken down into subdimensions. For example, there could be separate tables for brands and product categories, and each row in the dim_product table could reference the brand and category as foreign keys, rather than storing them as strings in the dim_product table. ==*Snowflake schemas are more normalized than star schemas, but star schemas are often preferred because they are simpler for analysts to work with.*==

#### **Column-Oriented Storage**

Dimension tables are usually much smaller (millions of rows), so in this section we will concentrate primarily on storage of facts.

*In most OLTP databases, storage is laid out in a row-oriented fashion: all the values from one row of a table are stored next to each other. Document databases are similar: an entire document is typically stored as one contiguous sequence of bytes.*

*==The idea behind column-oriented storage is simple: don’t store all the values from one row together, but store all the values from each column together instead.==* If each column is stored in a separate file, a query only needs to read and parse those columns that are used in that query, which can save a lot of work. The column-oriented storage layout relies on each column file containing the rows in the same order. Thus, if you need to reassemble an entire row, you can take the 23rd entry from each of the individual column files and put them together to form the 23rd row of the table.

*==It becomes important to encode data very compactly, to minimize the amount of data that the query needs to read from disk.==* 

Besides only loading those columns from disk that are required for a query, we can further reduce the demands on disk throughput by compressing data. Fortunately, *==column-oriented storage often lends itself very well to compression==*. Depending on the data in the column, different compression techniques can be used. One technique that is particularly effective in data warehouses is *==bitmap encoding==*

*Often, the number of distinct values in a column is small compared to the number of rows (for example, a retailer may have billions of sales transactions, but only 100,000 distinct products). We can now take a column with n distinct values and turn it into n separate bitmaps: one bitmap for each distinct value, with one bit for each row. The bit is 1 if the row has that value, and 0 if not.*

*There are also various other compression schemes for different kinds of data, but we won’t go into them in detail—see [58] for an overview.*

[[Memory bandwidth and vectorized processing]] ?????

**Several different sort orders**

A clever extension of this idea was introduced in C-Store and adopted in the commercial data warehouse Vertica [61, 62]. Different queries benefit from different sort orders, ==*so why not store the same data sorted in several different ways*==? Data needs to be replicated to multiple machines anyway, so that you don’t lose data if one machine fails. You might as well store that redundant data sorted in different ways so that *when you’re processing a query, you can use the version that best fits the query pattern*.

Having multiple sort orders in a column-oriented store is a bit similar to having multiple secondary indexes in a row-oriented store. But the big difference is that the row-oriented store keeps every row in one place (in the heap file or a clustered index), and secondary indexes just contain pointers to the matching rows. In a column store, there normally aren’t any pointers to data elsewhere, only columns containing values.

#### Writing to Column-Oriented Storage

These optimisations make sense in data warehouses, because most of the load consists of large read-only queries run by analysts. *==Column-oriented storage, compression, and sorting all help to make those read queries faster. However, they have the downside of making writes more difficult.==*

LSM-trees approach: *all writes first go to an in-memory store, where they are added to a sorted structure and prepared for writing to disk*. It doesn’t matter whether the in-memory store is row-oriented or column-oriented. When enough writes have accumulated, they are merged with the column files on disk and written to new files in bulk. This is essentially what Vertica does.

Queries need to examine both the column data on disk and the recent writes in mem‐ ory, and combine the two. However, the query optimizer hides this distinction from the user. From an analyst’s point of view, data that has been modified with inserts, updates, or deletes is immediately reflected in subsequent queries.

#### Aggregation: Data Cubes and Materialized Views

Another aspect of data warehouses that is worth mentioning briefly is *==materialized aggregates==*. As discussed earlier, data warehouse queries often involve an aggregate function, such as COUNT, SUM, AVG, MIN, or MAX in SQL. If the same aggregates are used by many different queries, it can be wasteful to crunch through the raw data every time.  

One way of creating such a cache is a *==materialized view==*. In a relational data model, it is often defined like a standard (virtual) view: a table-like object whose contents are the results of some query. The difference is that a *materialized view is an actual copy of the query results, written to disk, whereas a virtual view is just a shortcut for writing queries.* When you read from a virtual view, the SQL engine expands it into the view’s underlying query on the fly and then processes the expanded query.

When the underlying data changes, a materialized view needs to be updated, because it is a denormalized copy of the data. *The database can do that automatically, but such updates make writes more expensive, which is why materialized views are not often used in OLTP databases.* In read-heavy data warehouses they can make more sense (whether or not they actually improve read performance depends on the individual case).
![[Pasted image 20230604132322.png]]

The advantage of a materialized data cube is that certain queries become very fast because they have effectively been precomputed.

The disadvantage is that a data cube doesn’t have the same flexibility as querying the raw data. Most data warehouses therefore try to keep as much raw data as possible, and use aggregates such as data cubes only as a performance boost for certain queries.

[References](https://www.notion.so/DB-6ce16768bb054da69228f5a40cdfa87a?pvs=4#953ebb8548a54a318b29ce9ec1fc7707 )

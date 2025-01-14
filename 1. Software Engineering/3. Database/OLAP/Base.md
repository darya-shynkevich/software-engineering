These queries are often written by business analysts, and feed into reports that help the management of a company make better decisions (business intelligence). In order to differentiate this pattern of using databases from transaction processing, it has been called online analytic processing (OLAP). The difference between OLTP and OLAP is not always clear-cut, but some typical characteristics are listed in
![Pasted image 20230604124305](../../../_Attachments/Pasted%20image%2020230604124305.png)
The OLTP systems are usually expected to be highly available and to process transactions with low latency, since they are often critical to the operation of the business. 

*A [[Data Warehouse]], by contrast, is a separate database that analysts can query, without affecting OLTP operations.* 

The **[[Data Warehouse]]** contains a read-only copy of the data in all the various OLTP systems in the company. Data is extracted from OLTP databases (using either a periodic data dump or a continuous stream of updates), transformed into an analysis-friendly schema, cleaned up, and then loaded into the [[Data Warehouse]]. This process of getting data into the warehouse is known as ***Extract–Transform–Load (ETL)***

![Pasted image 20230604124518](../../../_Attachments/Pasted%20image%2020230604124518.png)

#### **Stars and Snowflakes: Schemas for Analytics**

The name ***“star schema”*** comes from the fact that when the table relationships are visualized, the fact table is in the middle, surrounded by its dimension tables; the connections to these tables are like the rays of a star.
![Pasted image 20230604124825](../../../_Attachments/Pasted%20image%2020230604124825.png)

A variation of this template is known as the ***snowflake schema***, where dimensions are further broken down into subdimensions. For example, there could be separate tables for brands and product categories, and each row in the dim_product table could reference the brand and category as foreign keys, rather than storing them as strings in the dim_product table. ==*Snowflake schemas are more normalized than star schemas, but star schemas are often preferred because they are simpler for analysts to work with.*==

#### **Column-Oriented Storage**

Dimension tables are usually much smaller (millions of rows), so in this section we will concentrate primarily on storage of facts.

*In most OLTP databases, storage is laid out in a row-oriented fashion: all the values from one row of a table are stored next to each other. Document databases are similar: an entire document is typically stored as one contiguous sequence of bytes.*

*==The idea behind column-oriented storage is simple: don’t store all the values from one row together, but store all the values from each column together instead.==* If each column is stored in a separate file, a query only needs to read and parse those columns that are used in that query, which can save a lot of work. The column-oriented storage layout relies on each column file containing the rows in the same order. Thus, if you need to reassemble an entire row, you can take the 23rd entry from each of the individual column files and put them together to form the 23rd row of the table.

*==It becomes important to encode data very compactly, to minimize the amount of data that the query needs to read from disk.==* 

Besides only loading those columns from disk that are required for a query, we can further reduce the demands on disk throughput by compressing data. Fortunately, *==column-oriented storage often lends itself very well to compression==*. Depending on the data in the column, different compression techniques can be used. One technique that is particularly effective in data warehouses is *==bitmap encoding==*

*Often, the number of distinct values in a column is small compared to the number of rows (for example, a retailer may have billions of sales transactions, but only 100,000 distinct products). We can now take a column with n distinct values and turn it into n separate bitmaps: one bitmap for each distinct value, with one bit for each row. The bit is 1 if the row has that value, and 0 if not.*

*There are also various other compression schemes for different kinds of data, but we won’t go into them in detail—see [58] for an overview.*

[Memory bandwidth and vectorized processing](Memory%20bandwidth%20and%20vectorized%20processing.md) ?????

**Several different sort orders**

A clever extension of this idea was introduced in C-Store and adopted in the commercial [[Data Warehouse]] Vertica [61, 62]. Different queries benefit from different sort orders, ==*so why not store the same data sorted in several different ways*==? Data needs to be replicated to multiple machines anyway, so that you don’t lose data if one machine fails. You might as well store that redundant data sorted in different ways so that *when you’re processing a query, you can use the version that best fits the query pattern*.

Having multiple sort orders in a column-oriented store is a bit similar to having multiple secondary indexes in a row-oriented store. But the big difference is that the row-oriented store keeps every row in one place (in the heap file or a clustered index), and secondary indexes just contain pointers to the matching rows. In a column store, there normally aren’t any pointers to data elsewhere, only columns containing values.

#### Writing to Column-Oriented Storage

These optimisations make sense in data warehouses, because most of the load consists of large read-only queries run by analysts. *==Column-oriented storage, compression, and sorting all help to make those read queries faster. However, they have the downside of making writes more difficult.==*

LSM-trees approach: *all writes first go to an in-memory store, where they are added to a sorted structure and prepared for writing to disk*. It doesn’t matter whether the in-memory store is row-oriented or column-oriented. When enough writes have accumulated, they are merged with the column files on disk and written to new files in bulk. This is essentially what Vertica does.

Queries need to examine both the column data on disk and the recent writes in mem‐ ory, and combine the two. However, the query optimizer hides this distinction from the user. From an analyst’s point of view, data that has been modified with inserts, updates, or deletes is immediately reflected in subsequent queries.

#### Aggregation: Data Cubes and Materialized Views

Another aspect of data warehouses that is worth mentioning briefly is *==materialized aggregates==*. As discussed earlier, [[Data Warehouse]] queries often involve an aggregate function, such as COUNT, SUM, AVG, MIN, or MAX in SQL. If the same aggregates are used by many different queries, it can be wasteful to crunch through the raw data every time.  

One way of creating such a cache is a *==materialized view==*. In a relational data model, it is often defined like a standard (virtual) view: a table-like object whose contents are the results of some query. The difference is that a *materialized view is an actual copy of the query results, written to disk, whereas a virtual view is just a shortcut for writing queries.* When you read from a virtual view, the SQL engine expands it into the view’s underlying query on the fly and then processes the expanded query.

When the underlying data changes, a materialized view needs to be updated, because it is a denormalized copy of the data. *The database can do that automatically, but such updates make writes more expensive, which is why materialized views are not often used in OLTP databases.* In read-heavy data warehouses they can make more sense (whether or not they actually improve read performance depends on the individual case).
![Pasted image 20230604132322](../../../_Attachments/Pasted%20image%2020230604132322.png)

The advantage of a materialized data cube is that certain queries become very fast because they have effectively been precomputed.

The disadvantage is that a data cube doesn’t have the same flexibility as querying the raw data. Most data warehouses therefore try to keep as much raw data as possible, and use aggregates such as data cubes only as a performance boost for certain queries.

# References:

1. [What You Always Wanted to Know About Datalog (And Never Dared to Ask)](https://www.researchgate.net/profile/Letizia-Tanca/publication/3296132_What_you_Always_Wanted_to_Know_About_Datalog_And_Never_Dared_to_Ask/links/0fcfd50ca2d20473ca000000/What-you-Always-Wanted-to-Know-About-Datalog-And-Never-Dared-to-Ask.pdf) (paper)
2. [Analytics for a SaaS startup: first year with Snowflake](https://www.youtube.com/watch?v=N1vqrRBi63w&list=PLdMXteIaGViJFoRUOoPjYaNqZFJY64TYr) (video)
3. Surajit Chaudhuri and Umeshwar Dayal: “[An Overview of Data Warehousing and OLAP Technology](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/sigrecord.pdf),” ACM SIGMOD Record, volume 26, number 1, pages 65–74, March 1997. [doi:10.1145/248603.248616](http://dx.doi.org/10.1145/248603.248616)
4. Ralph Kimball and Margy Ross: The Data Warehouse Toolkit: The Definitive Guide to Dimensional Modeling, 3rd edition. John Wiley & Sons, July 2013. ISBN: 978-1-118-53080-1 Derrick Harris: “[Why Apple, eBay, and Walmart Have Some of the Biggest Data Warehouses You’ve Ever Seen](http://gigaom.com/2013/03/27/why-apple-ebay-and-walmart-have-some-of-the-biggest-data-warehouses-youve-ever-seen/),” [gigaom.com](http://gigaom.com/), March 27, 2013.
5. Daniel J. Abadi, Peter Boncz, Stavros Harizopoulos, et al.: “[The Design and Implementation of Modern Column-Oriented Database Systems](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-column-stores.pdf),” Foundations and Trends in Databases, volume 5, number 3, pages 197–280, December 2013. [doi:10.1561/1900000024](http://dx.doi.org/10.1561/1900000024) [58]
6. Michael Stonebraker, Daniel J. Abadi, Adam Batkin, et al.: “[C-Store: A Column-oriented DBMS](http://www.cs.umd.edu/~abadi/vldb.pdf),” at 31st International Conference on Very Large Data Bases (VLDB), pages 553–564, September 2005. [61]
7. Andrew Lamb, Matt Fuller, Ramakrishna Varadarajan, et al.: “[The Vertica Analytic Database: C-Store 7 Years Later](http://vldb.org/pvldb/vol5/p1790_andrewlamb_vldb2012.pdf),” Proceedings of the VLDB Endowment, volume 5, number 12, pages 1790–1801, August 2012. [62]
8. Julien Le Dem and Nong Li: “[Efficient Data Storage for Analytics with Apache Parquet 2.0](http://www.slideshare.net/julienledem/th-210pledem),” at Hadoop Summit, San Jose, June 2014.
9. Jim Gray, Surajit Chaudhuri, Adam Bosworth, et al.: “[Data Cube: A Relational Aggregation Operator Generalizing Group-By, Cross-Tab, and Sub-Totals](http://arxiv.org/pdf/cs/0701155.pdf),” Data Mining and Knowledge Discovery, volume 1, number 1, pages 29–53, March 2007. [doi:10.1023/A:1009726021843](http://dx.doi.org/10.1023/A:1009726021843)
A NoSQL database, often referred to as a “Not Only SQL” database, is a non-relational database system designed to efficiently handle the storage and retrieval of unstructured or semi-structured data.

[NoSQL](https://github.com/donnemartin/system-design-primer#nosql) is a collection of data items represented in a key-value store, document store, wide column store, or a graph database.
#### [Why to choose NoSQL?](Why%20to%20choose%20NoSQL?.md)

(-) **lack of standardisation:** NoSQL doesn’t follow any specific standard, like how relational databases follow relational algebra. Porting applications from one type of NoSQL database to another might be a challenge;
(-) **consistency:** we won’t have strong data integrity, like primary and referential integrities in a relational database. Data might not be strongly consistent but slowly converging using a weak model like eventual consistency;
#### [MapReduce Querying](../../OLAP/MapReduce%20Querying.md)

## Primary categories of NoSQL DB

![](../../../../_Attachments/Pasted%20image%2020240119184814.png)
#### [Document Database](Types/Document%20Databases/_Base.md):

Database stores data in a document-like fashion, often utilising formats such as JSON or BSON. *==Each document is autonomous and has the potential for a unique structure, facilitating the storage of heterogeneous data.==* Examples of document-based NoSQL databases include [[MongoDB]] and [[CouchDB]], both of which provide versatility and suitability for use cases involving diverse data structures.
#### [Key-Value Database](Types/Key-Value%20Databases/_Base.md)

*==Data is stored as key-value pairs, where each key serves as an exclusive identifier linked to a value carrying relevant information.==* Key-value databases excel in straightforward read-and-write operations, with ease of scalability. Redis and Amazon DynamoDB exemplify key-value NoSQL databases, renowned for their efficiency in managing simple data operations and adaptability to increased workloads.
#### [Columnar Databases](Types/Columnar%20Databases/_Base.md)

*==These databases organise data into groupings known as column families, which comprise interconnected columns.==* This design targets workloads necessitating intensive write operations and proficient querying using predefined row and column keys. Notable instances include Apache [[Cassandra]] and [[HBase]], representative of column family-oriented NoSQL databases engineered to handle distinct types of data handling demands.
#### [Graph Databases](Types/Graph%20Databases/_Base.md)

The graph-based category is tailored to managing and querying *==data with intricate relationships and interconnections, as seen in social networks or recommendation systems.==* Graph databases leverage nodes, edges, and attributes to model and store data, streamlining complex traversal and relational-based queries. [[Neo4j]] and [[Amazon Neptune]] are well-recognized examples of graph-based NoSQL databases, optimised for handling intricate relationship structures.

# References:

1. [NoSQL Distilled](https://martinfowler.com/books/nosql.html) (book)
2. [NoSQL Databases: An Overview](https://www.thoughtworks.com/insights/blog/nosql-databases-overview)
3. [Introduction to NoSQL databases](https://www.youtube.com/watch?v=xQnIN9bW0og&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=19) (video)
4. [Schemaless Data Structures](https://martinfowler.com/articles/schemaless/#implicit-schema)
5. [Schema-on-Read vs Schema-on-Write](https://www.slideshare.net/awadallah/schemaonread-vs-schemaonwrite)
6. [Log Structured Merge Tree](https://www.cs.umb.edu/~poneil/lsmtree.pdf)
7. [Sorted String Tables and Compaction Strategies](https://github.com/scylladb/scylla/wiki/SSTable-compaction-and-compaction-strategies)
8. [Leveled Compaction Cassandra](https://www.datastax.com/blog/leveled-compaction-apache-cassandra)
9. [Scylla DB Compaction](https://github.com/scylladb/scylla/wiki/SSTable-compaction-and-compaction-strategies)
10. [NOSQL Patterns](http://horicky.blogspot.com/2009/11/nosql-patterns.html)
11. [Остаться в живых. Крупный проект на одной NoSQL](https://www.youtube.com/watch?v=ZLOFOxsDJIY&list=PLH-XmS0lSi_yVB6gNPkgA_ziD70q_8JFC&index=17) (video)
12. [5 Tips For Scaling NoSQL Databases: Don’t Trust Assumptions](http://highscalability.com/blog/2014/9/24/5-tips-for-scaling-nosql-databases-dont-trust-assumptionstes.html)
13. [An Unorthodox Approach to Database Design : The Coming of the Shard](http://highscalability.com/blog/2009/8/6/an-unorthodox-approach-to-database-design-the-coming-of-the.html)
14. [The Anatomy Of Search Technology: Blekko’s NoSQL Database](http://highscalability.com/blog/2012/4/25/the-anatomy-of-search-technology-blekkos-nosql-database.html)
15. [Google Spanner's Most Surprising Revelation: NoSQL is Out and NewSQL is In](http://highscalability.com/blog/2012/9/24/google-spanners-most-surprising-revelation-nosql-is-out-and.html)
16. [Schema Design for Time Series Data in MongoDB](https://www.mongodb.com/blog/post/schema-design-for-time-series-data-in-mongodb)
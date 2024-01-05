
1. A need for greater scalability than relational databases can easily achieve, including very large datasets or very high write throughput
2. A widespread preference for free and open source software over commercial database products
3. Specialized query operations that are not well supported by the relational model
4. Frustration with the restrictiveness of relational schemas, and a desire for a more dynamic and expressive data model
###### Which data model leads to simpler application code?

If the data in your application has a ***document-like structure*** (i.e., a tree of one-to- many relationships, where typically the entire tree is loaded at once) => document model. The relational technique of *shredding* — splitting a document-like structure into multiple tables — can lead to cumbersome schemas and unnecessarily complicated application code.

The poor support for joins in document databases may or may not be a problem, depending on the application. *For example, many-to-many relationships may never be needed in an analytics application that uses a document database to record which events occurred at which time.*

***For highly interconnected data, the document model is awkward, the relational model is acceptable, and graph models are the most natural.***

###### Schema flexibility in the document model

Document databases are sometimes called ***schemaless***, but that’s misleading, as the code that reads the data usually assumes some kind of structure  —i.e., there is an implicit schema, but it is not enforced by the database. A more accurate term is *==schema-on-read==* (the structure of the data is implicit, and only interpreted when the data is read), in contrast with *==schema-on-write==* (the traditional approach of relational databases, where the schema is explicit and the database ensures all written data conforms to it.

*Schema-on-read is similar to dynamic (runtime) type checking in programming languages, whereas schema-on-write is similar to static (compile-time) type checking.*

Schema changes have a bad reputation of being slow and requiring downtime. This reputation is not entirely deserved: most relational database systems execute the ALTER TABLE statement in a few milliseconds. MySQL is a notable exception — it copies the entire table on ALTER TABLE, which can mean minutes or even hours of downtime when altering a large table — although various tools exist to work around this limitation. 

*Running the UPDATE statement on a large table is likely to be slow on any database, since every row needs to be rewritten. If that is not acceptable, the application can leave first_name set to its default of NULL and fill it in at read time, like it would with a document database.*

***The schema-on-read approach is advantageous*** if the items in the collection don’t all have the same structure for some reason (i.e., the data is heterogeneous):
1. There are many different types of objects, and it is not practical to put each type of object in its own table.
2. The structure of the data is determined by external systems over which you have no control and which may change at any time.

###### Data locality for queries

*If your application often needs to access the entire document (for example, to render it on a web page), there is a performance advantage* to this storage locality. If data is split across multiple tables, multiple index lookups are required to retrieve it all, which may require more disk seeks and take more time.

***The locality advantage only applies if you need large parts of the document at the same time.*** The database typically needs to load the entire document, even if you access only a small portion of it, which can be wasteful on large documents. On updates to a document, the entire document usually needs to be rewritten — only modifications that don’t change the encoded size of a document can easily be performed in place. ***For these reasons, it is generally recommended that you keep documents fairly small and avoid writes that increase the size of a document.*** ***==These performance limitations significantly reduce the set of situations in which document databases are useful.==***

*It’s worth pointing out that the idea of grouping related data together for locality is not limited to the document model: Google’s Spanner database offers the same locality properties in a relational data model, by allowing the schema to declare that a table’s rows should be interleaved (nested) within a parent table. Oracle allows the same, using a feature called multi-table index cluster tables. The column-family concept in the Bigtable data model (used in Cassandra and HBase) has a similar purpose of managing locality*.

# References:

1. [Тред про БД. SQL и noSQL](https://twitter.com/_abstractart/status/1645427803670601729?t=AJ8OMxAOEpJpQ-r6XiozhA&s=35)
2. [https://speakerdeck.com/conradirwin/mongodb-confessions-of-a-postgresql-lover](https://speakerdeck.com/conradirwin/mongodb-confessions-of-a-postgresql-lover)
3. [Navigating the Database Selection Process: Tips and Best Practices](https://medium.com/bytebytego-system-design-alliance/navigating-the-database-selection-process-tips-and-best-practices-c737c89dab5f)
4. [CP Databases and AP Databases](https://blog.andyet.com/2014/10/01/right-database)
5. [Реляционные базы данных или NoSQL](https://boosty.to/megdu_skobok/posts/3bbc0208-9c69-4189-94ce-c941cbb08a78?share=post_link)
6. [NoSQL Databases: a Survey and Decision Guidance](https://medium.baqend.com/nosql-databases-a-survey-and-decision-guidance-ea7823a822d#.wskogqenq)
7. [Hilarious Video: Relational Database Vs NoSQL Fanbois](http://highscalability.com/blog/2010/9/5/hilarious-video-relational-database-vs-nosql-fanbois.html)
8. [Сравнение SQL- и NoSQL-баз данных](https://habr.com/ru/companies/ruvds/articles/727474/)
9. [SQL vs NoSQL](https://www.upwork.com/hiring/data/sql-vs-nosql-databases-whats-the-difference/)
10. [SQL or NoSQL](https://github.com/donnemartin/system-design-primer#sql-or-nosql)
11. [SQL vs NoSQL - Lesson Learned at Salesforce](https://engineering.salesforce.com/sql-or-nosql-9eaf1d92545b)
12. [SQL vs NO-SQL Databases — Everything you need to know](https://medium.com/javarevisited/sql-vs-no-sql-databases-everything-you-need-to-know-b723457446a5)
13. [ACID and BASE](https://neo4j.com/blog/acid-vs-base-consistency-models-explained/)
14. [ACID vs BASE: Comparison of two Design Philosophies](https://luminousmen.com/post/acid-vs-base-comparison-of-two-design-philosophies)
15. [Postgres vs Mongo](https://www.youtube.com/watch?v=SNzOZKvFZ68) (video)
16. [MongoDB vs. Postgres Benchmarks](https://www.youtube.com/watch?v=-AIjKrWi0x0&list=PLH-XmS0lSi_zTZrols83QSxI3Q96dSbBm&index=103) (video)
17. [Why MongoDB is ‘fundamentally better’ for developers](https://www.infoworld.com/article/3570729/why-mongodb-is-fundamentally-better-for-developers.amp.html)
18. [Why You Should Never Use MongoDB](http://www.sarahmei.com/blog/2013/11/11/why-you-should-never-use-mongodb/)
19. [Подумываете об использовании MongoDB?](https://habr.com/ru/company/otus/blog/565700/)
20. [NoSQL Databases: Survey and Decision Guidance](https://medium.baqend.com/nosql-databases-a-survey-and-decision-guidance-ea7823a822d)
21. [35+ Use Cases For Choosing Your Next NoSQL Database](http://highscalability.com/blog/2011/6/20/35-use-cases-for-choosing-your-next-nosql-database.html)
22. [101 Questions To Ask When Considering A NoSQL Database](http://highscalability.com/blog/2011/6/15/101-questions-to-ask-when-considering-a-nosql-database.html)
23. [What The Heck Are You Actually Using NoSQL For?](http://highscalability.com/blog/2010/12/6/what-the-heck-are-you-actually-using-nosql-for.html)
24. [Seven Signs You May Need A NoSQL Database](http://highscalability.com/blog/2010/2/16/seven-signs-you-may-need-a-nosql-database.html)
25. **[MySQL vs PostgreSQL in 2023.](https://dbconvert.com/blog/mysql-vs-postgresql/)**
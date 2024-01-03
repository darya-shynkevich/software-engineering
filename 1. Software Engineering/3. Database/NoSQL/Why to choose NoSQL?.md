
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


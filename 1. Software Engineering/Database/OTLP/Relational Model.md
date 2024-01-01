A key insight of the relational model was this: *you only need to build a query optimizer once, and then all applications that use the database can benefit from it.* If you don’t have a query optimizer, it’s easier to handcode the access paths for a particular query than to write a general-purpose optimizer—but the general-purpose solution wins in the long run.

[[1. Software Engineering/Database/OTLP/Base]]
[[1. Software Engineering/Database/OLAP/Base]]

### [[Why to choose NoSQL?]]

When it comes to representing *==many-to-one and many-to-many relation‐ ships,==* *==relational and document databases are not fundamentally different==*: in both cases, the related item is referenced by a unique identifier, which is called a *foreign key* in the relational model and a *document reference* in the document model.

*The main arguments in favor of the document data model are schema flexibility, better performance due to locality, and that for some applications it is closer to the data structures used by the application.* 

*The relational model counters by providing better support for joins, and many-to-one and many-to-many relationships.*

### Query Languages for Data

When the relational model was introduced, it included a new way of querying data: SQL is a declarative query language, whereas IMS and CODASYL queried the database using imperative code: [[Imperative and declarative languages]].

### [[MapReduce Querying]]





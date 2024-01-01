A NoSQL database, often referred to as a “Not Only SQL” database, is a non-relational database system designed to efficiently handle the storage and retrieval of unstructured or semi-structured data.
#### [[Why to choose NoSQL?]]

Different applications have different requirements, and the best choice of technology for one use case may well be different from the best choice for another use case. It therefore seems likely that in the foreseeable future, relational databases will continue to be used alongside a broad variety of nonrelational datastores—an idea that is sometimes called [[Polyglot persistence]].
#### [[MapReduce Querying]]

## Primary categories of NoSQL DB

#### [[Document database]]:

Database stores data in a document-like fashion, often utilizing formats such as JSON or BSON. *==Each document is autonomous and has the potential for a unique structure, facilitating the storage of heterogeneous data.==* Examples of document-based NoSQL databases include MongoDB and Couchbase, both of which provide versatility and suitability for use cases involving diverse data structures.
#### [[Key-Value Pair Databases]]

*==Data is stored as key-value pairs, where each key serves as an exclusive identifier linked to a value carrying relevant information.==* Key-value databases excel in straightforward read-and-write operations, with ease of scalability. Redis and Amazon DynamoDB exemplify key-value NoSQL databases, renowned for their efficiency in managing simple data operations and adaptability to increased workloads.
#### [[Column Family Databases]]

*==These databases organize data into groupings known as column families, which comprise interconnected columns.==* This design targets workloads necessitating intensive write operations and proficient querying using predefined row and column keys. Notable instances include Apache Cassandra and HBase, representative of column family-oriented NoSQL databases engineered to handle distinct types of data handling demands.
#### [[Graph-Based Databases]]

The graph-based category is tailored to managing and querying *==data with intricate relationships and interconnections, as seen in social networks or recommendation systems.==* Graph databases leverage nodes, edges, and attributes to model and store data, streamlining complex traversal and relational-based queries. Neo4j and Amazon Neptune are well-recognized examples of graph-based NoSQL databases, optimized for handling intricate relationship structures.
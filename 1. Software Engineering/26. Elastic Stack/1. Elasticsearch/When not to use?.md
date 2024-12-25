## 1. Handling relational datasets

[[Elasticsearch]], unlike databases such as [[MySQL]], was not designed to handle relational data. [[Elasticsearch]] allows you to have simple relationships in your data, such as parentchild and nested relationships, at a performance cost (at search time and indexing time, respectively). Data on [[Elasticsearch]] must be de-normalized (duplicating or adding redundant fields to documents, to avoid having to join data) to improve search and indexing/update performance.

## 2. Performing ACID transactions

[[Elasticsearch]] does not have the concept of transactions, so it does not offer ACID transactions.

At the individual request level, ACID properties can be achieved as follows:
1. **Atomicity** is achieved by sending a write request, which will either succeed on all active shards or fail. There is no way for the request to partially succeed. 
2. **Consistency** is achieved by writing to the primary shard. Data replication happens synchronously before a success response is returned. This means that all the read requests on all shards after a write request will see the same response. 
3. **Isolation** is offered since concurrent writes or updates (which are deletes and writes) can be handled successfully without any interference. 
4. **Durability** is achieved since once a document is written into Elasticsearch, it will persist, even in the case of a system failure. *Writes on [[Elasticsearch]] are not immediately persisted onto Lucene segments on disk as Lucene commits are relatively expensive operations. Instead, documents are written to a transaction log (referred to as a `translog`) and flushed into the disk periodically. If a node crashes before the data is flushed, operations from the translog will be recovered into the Lucene index upon startup.*

> In the case of relational data or ACID transaction requirements, [[Elasticsearch]] is often used alongside a traditional [RDBMS](1.%20Software%20Engineering/3.%20Database/OTLP/SQL/_Base.md) solution such as [[MySQL]]. In such architectures, the [RDBMS](1.%20Software%20Engineering/3.%20Database/OTLP/SQL/_Base.md) would act as the source of truth and handle writes/updates from the application. These updates can then be replicated to [[Elasticsearch]] using tools such as [[Logstash]] for fast/relevant searches and visualization/analytics use cases.


> Secondary indexes don’t map neatly to partitions.
## Partitioning Secondary Indexes by Document

**The gist**: In this indexing approach, each partition is completely separate: each partition maintains its own secondary indexes, covering only the documents in that partition. It doesn’t care what data is stored in other partitions. Whenever you need to write to the database — to add, remove, or update a document — you only need to deal with the partition that contains the document ID that you are writing. For that reason, a document-partitioned index is also known as a `local index`.

**Pros:**
(+) easy writes

**Cons:**
(-) complicated reads: need to query to all partitions, and combine all the results you get back.

> The approach is widely used: [MongoDB](MongoDB.md), [Riak](Riak), [Cassandra](Cassandra.md), [Elasticsearch](Elasticsearch), [[SolrCloud]], and [[VoltDB]] all use document-partitioned secondary indexes.

## Partitioning Secondary Indexes by Term

**The gist**: rather than each partition having its own secondary index (a local index), we can construct a global index that covers data in all partitions. However, we can’t just store that index on one node, since it would likely become a bottleneck and defeat the pur‐ pose of partitioning. A global index must also be partitioned, but it can be partitioned differently from the primary key index.


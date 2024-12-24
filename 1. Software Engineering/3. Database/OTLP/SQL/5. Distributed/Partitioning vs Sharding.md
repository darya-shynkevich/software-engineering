> ***Terminological confusion*** : *What we call a partition here is called a shard in MongoDB, Elasticsearch, and SolrCloud; it’s known as a region in HBase, a tablet in Bigtable, a vnode in Cassandra and Riak, and a vBucket in Couchbase. However, partitioning is the most established term, so we’ll stick with that.*

1. **Horizontal Partitioning** split big table into multiple tables in the same DB, client is agnostic
	1.  table name changes (or schema)
2. **Sharding** splits big tables into multiple tables across multiple DB servers
	1. everything is the same but server changes

# References:

1. [Designing Data-Intensive Applications](https://amzn.to/3t9LWvp?ref=workingsoftware.dev)
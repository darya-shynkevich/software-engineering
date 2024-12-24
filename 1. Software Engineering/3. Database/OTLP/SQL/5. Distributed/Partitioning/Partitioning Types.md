### 1. By Ranges (Dates, ids, ...)

**Pros:**
(+) Allow to archive unused data (Example: if we partitioned by date)

**Cons:**
(-) Certain access patterns can lead to hot spots: *If the key is a timestamp => the partition will be overwhelmed because of writes* => need to use something other than the timestamp as the first element of the key (prefix the name / uuid)
(-) Performance degradation if need to query all partitions (example: when you want to fetch the values of multiple entities within a time range, you need to perform a separate range query for each name / uuid)

> This partitioning strategy is used by [[Bigtable]], its open source equivalent [HBase](HBase.md), [[RethinkDB]], and [MongoDB](MongoDB.md) before version 2.4

*Data is divided into chunks depending on the ranges of a certain value in range-based partitioning.*
![Pasted image 20230605125424](../../../../../../_Attachments/Pasted%20image%2020230605125424.png)
*This requires a look up table to see where the data should be stored. Maintaining the consistency of such a table and obviously picking the ranges here are critical.

*When selecting a shard key for this partitioning type, it is essential to select one with high cardinality, so the number of possible values for that key is numerous. For example, a key with possible values of North, South, East, and West is of low cardinality since there are only 4. Coupled with that you would ideally prefer a good distribution within that cardinality.*

![Pasted image 20230605125531](../../../../../../_Attachments/Pasted%20image%2020230605125531.png)

Examples:
1. an application that stores data from a network of sensors, where the key is the timestamp of the measurement (year-month-day-hour-minute-second). Range scans are very useful in this case, because they let you easily fetch, say, all the readings from a particular month.

### 2. By List (Discrete values)

Example: 
1. states CA, AL
2. zip codes
3. countries

### 3. By Hash ([Consistent Hashing](../../../../../2.%20Architecture/1.%20Concepts/Consistent%20Hashing.md))

**Pros:**
(+) more even distribution when a suitable partition key isn't available and location can be calculated on the fly if you have the proper partition key.

**Cons:**
(-) resharding data can be difficult and maintaining consistency while being available is even harder
(-) we lose a nice property of key-range partitioning: the ability to do efficient range queries.
	-> in [MongoDB](MongoDB.md), if you have enabled hash-based sharding mode, any range query has to be sent to all partitions
	-> range queries on the primary key are not supported by [Riak](Riak), [[Couchbase]], or [Voldemort](Voldemort).
	-> [Cassandra](Cassandra.md) achieves a compromise: a table in Cassandra can be declared with a compound primary key consisting of several columns. Only the first part of that key is hashed to determine the partition, but the other columns are used as a concatenated index for sorting the data in [Cassandra](Cassandra.md)’s SSTables. A query therefore cannot search for a range of values within the first column of a compound key, but if it specifies a fixed value for the first column, it can perform an efficient range scan over the other columns of the key. Example: `(user_id, update_timestamp)` => efficiently retrieve all updates made by a particular user within some time interval, sorted by timestamp. Different users may be stored on different partitions, but within each user, the updates are stored ordered by timestamp on a single partition.

*Algorithmically partitioned databases use a hash function to locate data. This allows us given a specific partition key to find the correct physical shard to request the data from. Data is only distributed by the hash function. It doesn't take the size of the payload or space usage into account.*
![Pasted image 20230605125044](../../../../../../_Attachments/Pasted%20image%2020230605125044.png)
> *For partitioning purposes, the hash function need not be **cryptographically strong**. Cassandra and MongoDB use MD5, and Voldemort uses the Fowler– Noll–Vo function. Many programming languages have simple hash functions built in (as they are used for hash tables), but they may not be suitable for partitioning: for example, in Java’s Object.hashCode() and Ruby’s Object#hash, **the same key may have a different hash value in different processes***

Notes:
* In proxies ip_hash is used to understand to which backed we should go

##### Relationship Based Sharding

Relationship based sharding groups related data together on one physical shard to improve consistency and reduce queries across multiple shards. For instance, an application like Instagram would shard a user's posts and comments to the same node. It maximizes the benefits of a hash partition by clustering same entities together.
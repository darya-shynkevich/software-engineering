### 1. By Ranges (Dates, ids, ...)

*Data is divided into chunks depending on the ranges of a certain value in range-based partitioning.*
![[Pasted image 20230605125424.png]]
*This requires a look up table to see where the data should be stored. Maintaining the consistency of such a table and obviously picking the ranges here are critical.

*When selecting a shard key for this partitioning type, it is essential to select one with high cardinality, so the number of possible values for that key is numerous. For example, a key with possible values of North, South, East, and West is of low cardinality since there are only 4. Coupled with that you would ideally prefer a good distribution within that cardinality.*

![[Pasted image 20230605125531.png]]

(+) Allow to archive unused data (Example: if we partitioned by date)

### 2. By List (Discrete values)

Example: 
1. states CA, AL
2. zip codes
3. countries

### 3. By Hash ([[Consistent Hashing]])

*Algorithmically partitioned databases use a hash function to locate data. This allows us given a specific partition key to find the correct physical shard to request the data from.*

*Data is only distributed by the hash function. It doesn't take the size of the payload or space usage into account. Benefits of hashing allows a more even distribution when a suitable partition key isn't available and location can be calculated on the fly if you have the proper partition key.*
![[Pasted image 20230605125044.png]]
*Downside to such a partitioning strategy is that resharding data can be difficult and maintaining consistency while being available is even harder.*

*Resharding can be required when new data is added, old data is removed, or when the distribution of data across shards is not balanced. Resharding requires careful planning and execution to avoid data loss or system downtime, and can be difficult to do without impacting system performance. 

Another challenge is maintaining consistency while still making the system available for read and write operations. When data is distributed across multiple shards, it can become difficult to ensure that changes made on one shard are immediately reflected on all other shards. Without careful planning and implementation, this can lead to data inconsistencies and errors.*

Notes:
* Cassandra used it
* In proxies ip_hash is used to understand to which backed we should go
* Very popular

##### Relationship Based Sharding

Relationship based sharding groups related data together on one physical shard to improve consistency and reduce queries across multiple shards. For instance, an application like Instagram would shard a user's posts and comments to the same node. It maximizes the benefits of a hash partition by clustering same entities together.
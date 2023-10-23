![[Pasted image 20230605121444.png]]

Simply put, *==**sharding** is a method for distributing data across multiple machines==*. Sharding becomes especially handy when no single machine can handle the expected workload.

*Sharding is an example of horizontal scaling, while vertical scaling is an example of just getting larger and larger machines to support the new workload.*
![[Pasted image 20230605121659.png]]
You can also *==partition data==* in a few ways and *move specific tables to their databases*, much akin to what you see in microservice architectures, where a particular aspect of your application has its database server.
![[Pasted image 20230605121902.png]]
*More modern databases like Cassandra and others abstract that away from the application logic and its maintained at the database level.*

##### What are my options before sharding?

Like any distributed architecture, ***database sharding costs money***. Setting up shards, keeping the data on each shard up to date, and making sure requests are sent to the right shards is ***time-consuming and complicated***.

1. **Do nothing**: If it isn't broken, don't fix it.
2. **Vertical Scaling**: just get machines with more resources, adding additional RAM, adding more CPU cores for computationally heavy workloads, and adding additional storage. These are all options that don't require redesign of both your application and databases architectures.  *Other eventual limits, such as bandwidth (network or internal to the system), can also force you into sharding*.
3. **Replication**: read performance can be improved by making more copies of the database. BUT: caches first. 
	   *Replication makes write-heavy workloads harder to handle because each write must be copied to every node. This can vary depending on the datastore some of them do it asynchronously while others might delay the initial write to ensure its been replicated.*
	   
	   ![[Pasted image 20230605124051.png]]
4. **Specialized Databases**: *Poor performance in a database can be improved by redesigning it to better suit its workload. For instance, using Elasticsearch for search data and S3 for blobs can be more effective than using a relational database. Outsourcing these functions may be preferable to sharding the entire database.*

*==A sharded database may be the best way to go if your application database manages a lot of data, needs a lot of reads and writes, and/or needs to be available at all times.==*

*==**Sharding allows for significant scalability, but comes with increased complexity, overhead, and infrastructure costs.***==

#### How does it work?

1. How do we distribute the data across shards? Are there potential hotspots if data isn't distributed evenly?
		*The term **hotspot** means that the workload of a node has exceeded the threshold with respect to certain resource (memory, io, and etc.).*
2. What queries do we run and how do the tables interact?
3. How will data grow? How will it need to be redistributed later?

**Shard Key** is a piece of the **primary key** that tells how the data should be distributed. With a shard key, you can quickly find and change data by routing operations to the correct database.

The same node houses entries that have the same shard key. A group of data that shares the same shard key is referred to as a **logical shard**. Multiple logical shards are included in a database node, also known as a **physical shard**.
![[Pasted image 20230605124814.png]]
*The most crucial presumption is also the one that is most difficult to alter in the future. A logical shard can only span one node because it is an atomic unit of storage. The database cluster is effectively out of space in the case where shard is too big for single node.*

##### Key Based Sharding

*Algorithmically sharded databases use a hash function to locate data. This allows us given a specific shard key to find the correct physical shard to request the data from.*

*Data is only distributed by the hash function. It doesn't take the size of the payload or space usage into account. Benefits of hashing allows a more even distribution when a suitable partition key isn't available and location can be calculated on the fly if you have the proper partition key.*
![[Pasted image 20230605125044.png]]
*Downside to such a sharding strategy is that resharding data can be difficult and maintaining consistency while being available is even harder.*

*Resharding can be required when new data is added, old data is removed, or when the distribution of data across shards is not balanced. Resharding requires careful planning and execution to avoid data loss or system downtime, and can be difficult to do without impacting system performance. 

Another challenge is maintaining consistency while still making the system available for read and write operations. When data is distributed across multiple shards, it can become difficult to ensure that changes made on one shard are immediately reflected on all other shards. Without careful planning and implementation, this can lead to data inconsistencies and errors.*

##### Range Based Sharding

*Data is divided into chunks depending on the ranges of a certain value in range-based sharding.*
![[Pasted image 20230605125424.png]]
*This requires a look up table to see where the data should be stored. Maintaining the consistency of such a table and obviously picking the ranges here are critical.

*When selecting a shard key for this sharding type, it is essential to select one with high cardinality, so the number of possible values for that key is numerous. For example, a key with possible values of North, South, East, and West is of low cardinality since there are only 4. Coupled with that you would ideally prefer a good distribution within that cardinality.*

![[Pasted image 20230605125531.png]]

##### Relationship Based Sharding

Relationship based sharding groups related data together on one physical shard to improve consistency and reduce queries across multiple shards. For instance, an application like Instagram would shard a user's posts and comments to the same node. It maximizes the benefits of a single partition by clustering same entities together.

#### Cross Shard Transactions

*==As a general rule the longer a transaction is open the more contention and potentially failure can occur.==*

Two-phase commit:
- Leader writes a durable transaction record indicating a cross-shard transaction.
- Participants write a permanent record of their willingness to commit and notify the leader.
- The leader commits the transaction by updating the durable transaction record after receiving all responses. (It can abort the transaction if no one responds.)
- Participants can show the new state after the leader announces the commit decision. (They delete the staged state if the leader aborts the transaction.)
      
    *Read-and-write amplification in the protocol path is a major issue. Write amplification occurs because you must write a transaction record and durably stage a commit, which requires at least one write per participant. Lock contention and application instability can result from excessive writes. ==The database must additionally filter every read to ensure that it does not see any state that is dependent on a pending cross-shard transaction, which affects all reads in the system, even non-transactional ones.*==


References:
1. https://architecturenotes.co/database-sharding-explained/ 
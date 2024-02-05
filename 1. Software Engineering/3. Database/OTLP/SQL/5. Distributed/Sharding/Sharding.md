![Pasted image 20230605121444](../../../../../../_Attachments/Pasted%20image%2020230605121444.png)

Sharding is a process of segmenting the data into shards that are spread on multiple DB instances => *==it is a method for distributing data across multiple machines==*. It becomes especially handy when no single machine can handle the expected workload.

*Sharding is an example of horizontal scaling, while vertical scaling is an example of just getting larger and larger machines to support the new workload.*
![Pasted image 20230605121659](../../../../../../_Attachments/Pasted%20image%2020230605121659.png)
*==A sharded database may be the best way to go if your application database manages a lot of data, needs a lot of reads and writes, and/or needs to be available at all times.==*

*==**Sharding allows for significant scalability, but comes with increased complexity, overhead, and infrastructure costs.***==
# [What are my options before?](What%20are%20my%20options%20before?.md)

# Pros of Sharding

(+) Scalability (horizontal scalability)
	Data
	Memory
(+) Security (users can access certain shards)
(+) Optimal and smaller index size
# Cons of Sharding

(-) Complex client (aware of the shard)
(-) Transactions across shards problem. 
	Almost impossible to do atomic multi-shard transaction
(-) Rollbacks
(-) Schema changes are hard
(-) JOINS across DBs
(-) Has to be something you know in the query
# Sharding Types:

1. How do we distribute the data across shards? Are there potential [Hotspot](../../../../../2.%20Architecture/1.%20Base/1.%20Concepts/Hotspot.md)s if data isn't distributed evenly?
2. What queries do we run and how do the tables interact?
3. How will data grow? How will it need to be redistributed later?
4. How many shards per database?
		Empirically, we can determine how much each node can serve with acceptable performance. It can help us find out the maximum amount of data that we would like to keep on any one node. For example, if we find out that we can put a maximum of 50 GB of data on one node, we have the following:
			Database size == 10 TB
			Size of a single shard == 50 GB
			Number of shards the database should be distributed in == 10 TB/50 GB == 200 shards

**Shard Key** is a piece of the **primary key** that tells how the data should be distributed. With a shard key, you can quickly find and change data by routing operations to the correct database.

The same node houses entries that have the same shard key. A group of data that shares the same shard key is referred to as a **logical shard**. Multiple logical shards are included in a database node, also known as a **physical shard**.
## 1. Key-range based sharding

Tables (or subtables) that belong to the same **shard key** are distributed to one database shard. The following figure shows that several tables with the same shard key are placed in a single database shard:

![](../../../../../../_Attachments/Pasted%20image%2020240119235320.png)
The basic design techniques used in multi-table sharding are as follows:
1. There’s a shard key in the `Customer` mapping table. This table resides on each shard and stores the shard keys used in the shard. Applications create a mapping logic between the shard keys and database shards by reading this table from all shards to make the mapping efficient. Sometimes, applications use advanced algorithms to determine the location of a shard key belonging to a specific shard.
2. The shard key column, `Customer_Id`, is replicated in all other tables as a data isolation point. It has a trade-off between an impact on increased storage and locating the desired shards efficiently. Apart from this, it’s helpful for data and workload distribution to different database shards. The data routing logic uses the shard key at the application tier to map queries specified for a database shard.
3. Primary keys are unique across all database shards to avoid key collision during data migration among shards and the merging of data in the online analytical processing (OLAP) environment.
4. The column `Creation_date` serves as the data consistency point, with an assumption that the clocks of all nodes are synchronised. This column is used as a criterion for merging data from all database shards into the global view when essential.

(+) Using key-range-based sharding method, the range-query-based scheme is easy to implement. We precisely know where (which node, which shard) to look for a specific range of keys.
(+) Range queries can be performed using the sharding keys, and those can be kept in shards in sorted order. How exactly such a sorting happens over time as new data comes in is implementation specific.

(-) Range queries can’t be performed using keys other than the sharding key.
(-) If keys aren’t selected properly, some nodes may have to store more data due to an uneven distribution of the traffic.

## 2. Hash-based sharding

 The main concept is to use a hash function on the key to get a hash value and then mod by the number of shards. Once we’ve found an appropriate hash function for keys, we may give each shard a range of hashes (rather than a range of keys). Any key whose hash occurs inside that range will be kept in that shard.

`f = Value mod 4`, where `n` is the number of nodes, which is four. We allocate keys to nodes by checking the `mod` for each key. Keys with a mod value of 2 are allocated to node 2. Keys with a mod value of 1 are allocated to node 1. Keys with a mod value of 3 are allocated to node 3. Because there’s no key with a mod value of 0, node 0 is left vacant.

![](../../../../../../_Attachments/Pasted%20image%2020240120000053.png)

(+) Keys are uniformly distributed across the nodes.

(-) We can’t perform range queries with this technique. Keys will be spread over all shards.
## [Consistent Hashing](../../../../../2.%20Architecture/1.%20Base/1.%20Concepts/Consistent%20Hashing.md)

# Rebalance the shards

- The distribution of the data isn’t equal.
- There’s too much load on a single shard.
- There’s an increase in the query traffic, and we need to add more nodes to keep up.

## Solutions

1. Use techniques from [Consistent Hashing](../../../../../2.%20Architecture/1.%20Base/1.%20Concepts/Consistent%20Hashing.md). Using [[Consistent Hashing]] ring it is easy to add new node. 
2. **Fixed numbers of shards:** In this approach, the number of shards to be created is fixed at the time when we set our database up. We create a higher number of shards than the nodes and assign these shards to nodes. So, when a new node is added to the system, it can take a few shards from the existing nodes until the shards are equally divided.
	   (-) the size of each shard grows with the total amount of data in the cluster since all the shards contain a small part of the total data. If a shard is very small, it will result in too much overhead because we may have to make a large number of small-sized shards, each costing us some overhead. If the shard is very large, rebalancing the nodes and recovering from node failures will be expensive. It’s very important to choose the right number of shards. 
	   ! A fixed number of shards is used in Elasticsearch, Riak, and many more.
2. **Dynamic sharding:** In this approach, when the size of a shard reaches the threshold, it’s split equally into two shards. One of the two split shards is assigned to one node and the other one to another node. In this way, the load is divided equally. The number of shards adapts to the overall data amount, which is an advantage of dynamic sharding.
	   (-) it’s difficult to apply dynamic rebalancing while serving the reads and writes. Dynamic rebalancing during reads and writes is challenging because it involves moving data between nodes, causing latency and potential conflicts. Ensuring data consistency (as data is simultaneously moved and accessed) and availability (potentially requiring pauses in reads/writes during rebalancing) introduces complexities that can impact system performance and reliability. 
	   ! This approach is used in HBase and MongoDB.
3. **Shard proportionally to nodes:** In this approach, the number of shards is proportionate to the number of nodes, which means every node has fixed shards. While the number of nodes remains constant, the size of each shard rises according to the dataset size. However, as the number of nodes increases, the shards shrink. When a new node enters the network, it splits a certain number of current shards at random, then takes one half of the split and leaves the other half alone. This can result in an unfair split. This approach is used by Cassandra and Ketama.

# Request routing

- Allow the clients to request any node in the network. If that node doesn’t contain the requested data, it forwards that request to the node that does contain the related data.
- The second approach contains a routing tier. All the requests are first forwarded to the routing tier, and it determines which node to connect to fulfill the request.
- The clients already have the information related to partitioning and which partition is connected to which node. So, they can directly contact the node that contains the data they need.

# [Cross Shard Transactions](Cross%20Shard%20Transactions.md)

# References:

1. ~~[Database Sharding Crash Course (with Postgres examples)](!https://www.youtube.com/watch?v=d1fXBLqnFvc&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=17)~~
2. ~~[Avoid premature Database Sharding](https://www.youtube.com/watch?v=aXD4tWbkoJo&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=17) (video)~~
3. ~~[When should you shard your database?](https://www.youtube.com/watch?v=iHNovZUZM3A&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=40) (video)~~
4. [Database Sharding — System Design Interview Concept](https://medium.com/@anuupadhyay1994/database-sharding-system-design-interview-concept-2d0d6bdbb5dd)
5. [What is Database Sharding?](https://www.youtube.com/watch?v=zaRkONvyGr8&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=5) (video)
6. [What is Database Sharding](https://naveenpn.medium.com/what-is-database-sharding-d38237cf5a8f)
7. [Database Sharding Crash Course (with Postgres examples)](https://www.youtube.com/watch?v=d1fXBLqnFvc&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=16) (video)
8. [How Sharding Works](https://medium.com/@jeeyoungk/how-sharding-works-b4dec46b3f6)
9. [Распределенный SQL: альтернатива шардированию баз данных](https://habr.com/ru/companies/ruvds/articles/714322/)
10. [When should you shard your database?](https://www.youtube.com/watch?v=iHNovZUZM3A&list=PLQnljOFTspQXNP6mQchJVP3S-3oKGEuw9&index=39) (video)
11. [How Sharding Works](https://medium.com/@jeeyoungk/how-sharding-works-b4dec46b3f6)
12. [Database Sharding Explained](https://architecturenotes.co/database-sharding-explained/)
13. [System Design Fundamentals: Database Sharding](https://rabisiddique.medium.com/system-design-fundamentals-database-sharding-2240c897fe98)
14. **[All Things Sharding: Techniques and Real-Life Examples in NoSQL Data Storage Systems](https://kousiknath.medium.com/all-things-sharding-techniques-and-real-life-examples-in-nosql-data-storage-systems-3e8beb98830a)**
15. [Жизнь после шардинга](https://www.youtube.com/watch?v=ZGAHlGfW1yw) (video)
16. [How We Partitioned Airbnb’s Main Database in Two Weeks](https://medium.com/airbnb-engineering/how-we-partitioned-airbnb-s-main-database-in-two-weeks-55f7e006ff21)
17. [Troubles With Sharding - What Can We Learn From The Foursquare Incident?](http://highscalability.com/blog/2010/10/15/troubles-with-sharding-what-can-we-learn-from-the-foursquare.html)
18. [The Dark Side of Sharding: Mistakes You Can’t Afford to Make](https://levelup.gitconnected.com/the-dark-side-of-sharding-mistakes-you-cant-afford-to-make-7905a6c45499)
19. [Sharding The Hibernate Way](http://highscalability.com/blog/2008/7/26/sharding-the-hibernate-way.html)
20. [Sharding MySQL at Pinterest](https://medium.com/@Pinterest_Engineering/sharding-pinterest-how-we-scaled-our-mysql-fleet-3f341e96ca6f)
21. [Sharding MySQL at Twilio](https://www.twilio.com/engineering/2014/06/26/how-we-replaced-our-data-pipeline-with-zero-downtime)
22. [Sharding MySQL at Square](https://medium.com/square-corner-blog/sharding-cash-10280fa3ef3b)
23. [Sharding MySQL at Quora](https://www.quora.com/q/quoraengineering/MySQL-sharding-at-Quora)
24. [Sharding Layer of Schemaless Datastore at Uber](https://eng.uber.com/schemaless-rewrite/)
25. [Sharding & IDs at Instagram](https://instagram-engineering.com/sharding-ids-at-instagram-1cf5a71e5a5c)
26. [Sharding Postgres at Notion](https://www.notion.so/blog/sharding-postgres-at-notion)
27. [Solr: Improving Performance for Batch Indexing at Box](https://blog.box.com/blog/solr-improving-performance-batch-indexing/)
28. [Geosharded Recommendations (3 parts) at Tinder](https://medium.com/tinder-engineering/geosharded-recommendations-part-3-consistency-2d2cb2f0594b)
29. [Scaling Services with Shard Manager at Facebook](https://engineering.fb.com/production-engineering/scaling-services-with-shard-manager/)
**Key-value databases** use key-value methods like hash tables to store data in key-value pairs. We can see this depicted in the figure a couple of paragraphs below. Here, the key serves as a unique or primary key, and the values can be anything ranging from simple scalar values to complex objects. These databases allow easy partitioning and horizontal scaling of the data. 
Some popular key-value databases include 
1. [[Amazon DynamoDB]], 
2. [[Redis]], 
3. [[Memcached]] DB.

**Use case**: Key-value databases are efficient for session-oriented applications. Session oriented-applications, such as web applications, store users’ data in the main memory or in a database during a session. This data may include user profile information, recommendations, targeted promotions, discounts, and more. A unique ID (a key) is assigned to each user’s session for easy access and storage. Therefore, a better choice to store such data is the key-value database.

The following figure shows an example of a key-value database. The `Product ID` and `Type` of the item are collectively considered as the primary key. This is considered as a key for this key-value database. Moreover, the schema for storing the item attributes is defined based on the nature of the item and the number of attributes it possesses.

![](../../../../../../_Attachments/Pasted%20image%2020240119185154.png)
# References:

1. [**Key-Value Databases**](http://www.cs.ucsb.edu/~agrawal/fall2009/dynamo.pdf)
2. [**LevelDB** - Fast And Lightweight Key/Value Database From The Authors Of MapReduce And BigTable](http://highscalability.com/blog/2011/8/10/leveldb-fast-and-lightweight-keyvalue-database-from-the-auth.html)
3. **[How RocksDB works](https://artem.krylysov.com/blog/2023/04/19/how-rocksdb-works/)**
4. **[RocksDB — A Deep Dive into the Internals of an Embedded Key-Value Storage Engine](https://blog.devgenius.io/rocksdb-a-deep-dive-into-the-internals-of-an-embedded-key-value-storage-engine-3a88132523c2)**
5. [DynamoDB at Nike](https://medium.com/nikeengineering/becoming-a-nimble-giant-how-dynamo-db-serves-nike-at-scale-4cc375dbb18e)
6. [DynamoDB at Segment](https://segment.com/blog/the-million-dollar-eng-problem/)
7. [DynamoDB at Mapbox](https://blog.mapbox.com/scaling-mapbox-infrastructure-with-dynamodb-streams-d53eabc5e972)
8. [Manhattan: Distributed Key-Value Database at Twitter](https://blog.twitter.com/engineering/en_us/a/2014/manhattan-our-real-time-multi-tenant-distributed-database-for-twitter-scale.html)
9. [Sherpa: Distributed NoSQL Key-Value Store at Yahoo](https://yahooeng.tumblr.com/post/120730204806/sherpa-scales-new-heights)
10. [HaloDB: Embedded Key-Value Storage Engine at Yahoo](https://yahooeng.tumblr.com/post/178262468576/introducing-halodb-a-fast-embedded-key-value)
11. [MPH: Fast and Compact Immutable Key-Value Stores at Indeed](http://engineering.indeedblog.com/blog/2018/02/indeed-mph/)
12. [Venice: Distributed Key-Value Database at Linkedin](https://engineering.linkedin.com/blog/2017/02/building-venice-with-apache-helix)
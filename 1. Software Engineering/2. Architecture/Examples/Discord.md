Decision to store all user's messages forever

Arguably, one of the **best databases for iterating quickly is [MongoDB](../../3.%20Database/OTLP/NoSQL/Types/Document%20Databases/MongoDB.md)** => but they planned everything for easy migration to a new DB (they knew they were not going to use [MongoDB](../../3.%20Database/OTLP/NoSQL/Types/Document%20Databases/MongoDB.md) `sharding` because *it is complicated to use and not known for stability*

Up to 2015, the messages were stored in a ***MongoDB collection with a single compound index on `channel_id` and `created_at`***, but when they reached 100 million stored messages that approach stop working.

WHY?

*==The data and the index could no longer fit in RAM and latencies started to become unpredictable.==*

# Choosing the Right Database

### Application patterns:

1. Reads were extremely random and their read/write ratio was about 50/50
2. **Voice chat-heavy Discord servers** send almost no messages.
3. **Private text chat-heavy Discord servers** send a decent number of messages, easily reaching between 100 thousand to 1 million messages a year.
4. **Large public Discord servers** send a lot of messages. They have thousands of members sending thousands of messages a day and easily rack up millions of messages a year. They almost always are requesting messages sent in the last hour and they are requesting them often. Because of that the data is usually in the disk cache.
5. Even more random reads: viewing plus jumping to pinned messages, and full-text search.

### Application requirements:

- **Linear scalability —** They do not want to reconsider the solution later or manually re-shard the data.
- **Automatic failover —** They love sleeping at night and building Discord to self-heal as much as possible.
- **Low maintenance —** It should just work once they set it up. They should only have to add more nodes as data grows.
- **Proven to work —** They love trying out new technology, but not too new.
- **Predictable performance** **—** They have alerts go off when their API’s response time 95th percentile goes above 80ms. They also do not want to have to cache messages in Redis or Memcached.
- **Not a blob store —** Writing thousands of messages per second would not work great if they had to constantly deserialize blobs and append to them.
- **Open source —** They believe in controlling their own destiny and don’t want to depend on a third-party company.

=> [Cassandra](../../3.%20Database/OTLP/NoSQL/Types/Columnar%20Databases/Cassandra.md)

# Data Modeling

The Cassandra is a [Key Key Value Storage](Key%20Key%20Value%20Storage) (like Dynamo DB). The two keys are `primary key`. The first K is the `partition ke`y and is used to determine which node the data lives on and where it is found on the disk.

The `partition` contains multiple rows within it and a row within a `partition` is identified by the second K, which is the `clustering key`. The `clustering key` acts as both a `primary key` within the `partition` and how the rows are sorted.

`channel_id` = `partition key` since all queries operate on a channel, but `created_at` didn’t make a great clustering key because two messages can have the same creation time => then created a service that generated a unique `message_id` for every message.  The primary key became (`channel_id`, `message_id`),

```sql
CREATE TABLE messages (
	channel_id bigint, 
	message_id bigint, 
	author_id bigint, 
	content text, 
	PRIMARY KEY (channel_id, message_id)
)  
WITH CLUSTERING ORDER BY (message_id DESC)
```

BUT

> **You should not use too big partitions because it put a lot of GC pressure on Cassandra during compaction, cluster expansion, and more.** 

It became clear they had to somehow bind the size of partitions because a single Discord channel can exist for years and perpetually grow in size => They decided to bucket their messages by time. 

Cassandra partition keys can be compounded, so their new primary key became ((channel_id, bucket), message_id).

```sql
CREATE TABLE messages ( 
	channel_id bigint, 
	bucket int, 
	message_id bigint, 
	author_id bigint, 
	content text, 
	PRIMARY KEY ((channel_id, bucket), message_id)
)  
WITH CLUSTERING ORDER BY (message_id DESC);
```

- The downside of this method is that rarely active Discords will have to query multiple buckets to collect enough messages over time. In practice, this has proved to be fine because for active Discords enough messages are usually found in the first partition and they are the majority.

# Dark Launch

1. They set up their code to double read/write to MongoDB and Cassandra
2. However, there were bugs related to eventually consistency: user_1 edits a message and at the same time user_2 delete the same message, they ended up with a row that was missing all the data except the primary key and the text since all Cassandra writes are upserts.
	1. Write the whole message back when editing the message
	2. Figuring out that the message is corrupt and deleting it from the database (+)
	
	They went with the second option, which they did by choosing a column that was required (in this case author_id) and deleting the message if it was null.

	

# References

1. [How Discord Stores Billions of Messages — Big Surprises in System Design](https://interviewnoodle.com/how-discord-stores-billions-of-messages-big-surprises-in-system-design-e48fa07a2665)
Redis is more than just a cache. It runs in memory, thus providing faster reads and writes.

1. it can survive a crash with disk [persistence.](Redis%20Persistence%20Models.md)
2. it supports atomic operations using transactions.
3. it pffers high performance ([single-threaded](How%20Redis%20works%20internally.md), so there is no need for locks)

### **1. Caching**

Most likely the same data will often get queried from the database. So they set up a Redis cache in front of the database
	> faster response - 100x
	> reduced database load

Usually Redis is used in front of primary DB for cases when:
- data changes infrequently and is requested often
- data  is less mission-critical and is frequently evolving.
### 2. Queueing

They added extra servers to query and process data - named **query workers** => BUT some queries took longer to finish

So they set up Redis [streams](https://redis.io/docs/latest/develop/data-types/streams/) to queue requests. It allowed them to process requests asynchronously.

A Redis **stream** is a data structure that acts like an append-only log. This means every entry is immutable.

![](Pasted%20image%2020240623143703.png)
How it works:
1. The request gets forwarded on a cache miss
2. The request gets wrapped in a message and gets assigned a unique identifier
3. The message gets added to the queue
4. The query worker consumes the message if it has free capacity
5. The query worker interacts with the database

(+) buffer requests for asynchronous processing
(+) decouple serving and processing of requests
(+) auto-scale query workers based on load
### 3. Locking

Many expensive queries at once will overload the database.

So they installed a distributed [lock](https://redis.io/glossary/redis-lock/) using Redis.

A **distributed lock** orchestrates access to a shared resource from many clients.
![](Pasted%20image%2020240623143940.png)

How it works:
1. The query worker acquires a lock before talking to the database
2. An expiry time is set for the lock
3. The number of query workers that access the database at once is limited. While others must wait

(+) resilience by avoiding many queries at once
(+) avoid noisy neighbour problem

### 4. Throttling

There’s a risk of lock acquisition failure. It will overload the database.

So they use Redis [streams](https://redis.io/docs/latest/develop/data-types/streams/) to throttle messages.
![](Pasted%20image%2020240623144149.png)
How it works:
1. The query worker fails to acquire a lock, so the message gets added back to the queue
2. Also a delay gets assigned to the message before inserting it
3. And an extra delay gets added each time the same message is re-inserted into the queue - exponential backoff

(+) congestion management by controlling the number of parallel database requests

### 5. Session Store

The web server stores the user’s data and preferences.

Yet it’s hard to scale a _stateful_ web server.

So they installed a separate session store using Redis.

![](Pasted%20image%2020240623144259.png)

How it works:
1. Session data is stored in the Redis hash data structure
2. An expiry time is set for each user's data
3. The expiry time gets renewed whenever the user requests something

(+) scale _stateless_ web servers easily
(+) handle traffic spikes

### 6. Rate Limiting

![](Pasted%20image%2020240623144411.png)

How it works:
1. A [counter](https://redis.io/glossary/storing-counters-in-redis/) is implemented using the Redis hash data structure
2. The counter represents the number of requests allowed on each API endpoint in a period
3. The counter gets decremented with each request
4. Requests get rejected if the counter becomes 0
5. The counter gets reset to its initial value after a specific period

### 7. [Multi-model DB](https://www.youtube.com/watch?v=VLTPqImLapM&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=83)

Coupled with Redis plug-ins and its various High Availability (HA) setups, Redis as a database  has become incredibly useful for certain scenarios and workloads.
1. Atomicity: +
2. Isolation: serialiseble
3. Durability: AOF (fast and similar to WAL)
4. Consistency +
5. Availability + (depends on consistency)
6. Concurrency Control (Optimistic locks)

### 8. [Sorted sets](https://redis.io/docs/latest/develop/data-types/sorted-sets/) can be used to create a leaderboard

### 9. [HyperLogLog](https://redis.io/docs/latest/develop/data-types/probabilistic/hyperloglogs/) can be used to estimate the number of items in a set

### 10. [Pub-Sub](https://redis.io/docs/latest/develop/interact/pubsub/) can be used to implement real-time messaging

### 11. The [geospatial index](https://redis.io/glossary/geospatial-indexing/) can be used to find nearby points within a radius

### 12. [Time series](https://redis.io/docs/latest/develop/data-types/timeseries/) data type can be used for data analytics

### 13. The [list](https://redis.io/docs/latest/develop/data-types/lists/) data type can be used to create a social network timeline

### 14. The [RedisSearch](https://redis.io/search/) module supports full-text search and SQL-like query functionality. It’s useful if the search index fits in memory or changes often

### 15. The [RedisJSON](https://redis.io/json/) module can be used to store nested JSON data. It avoids de-serialization costs

## References:

1. ~~[Why Is Redis a Distributed Swiss Army Knife](https://newsletter.systemdesign.one/p/redis-use-cases?utm_source=substack&publication_id=1511845&post_id=145546875&utm_medium=email&utm_content=share&utm_campaign=email-share&triggerShare=true&isFreemail=true&r=1vxw4z&triedRedirect=true)~~
2. ~~[Can Redis be used as a Primary database?](https://www.youtube.com/watch?v=VLTPqImLapM&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=84)~~
### [Redis vs Memcached](Redis%20vs%20Memcached.md)

The main differentiating point between it and Redis is its lack of data types (**both the key and the value are strings**) and its limited eviction policy of just LRU (least recently used).

Redis is single-threaded while Memcached is **multithreaded**. 

Memcached has a client and server component, each of which is necessary to run the system. The system is designed in a way that half the logic is encompassed in the server, whereas the other half is in the client. However, each server follows the **shared-nothing architecture**. In this architecture, servers are unaware of each other, and there’s no synchronization, data sharing, and communication between the servers.

![](../../../../../../_Attachments/Pasted%20image%2020240127200120.png)

### **[Lease](Leases.md)**

Leases are used to address _stale_ sets (when web server writes a stale value in the cache) and _thundering herds_ (when a key undergoes heavy read and write operations)
![](Pasted%20image%2020240623184919.png)

1. The first request gets routed to Memcache. But it isn’t a normal GET operation instead _lease_ GET operation. Memcache gives client a lease (a 64-bit token bound to the requested key). To mitigate thundering herds, Memcache returns a token only once every 10 seconds per key
2. The Memcache forwards the request to PostgreSQL and asks the client to wait
3. If a second read request comes within 10 seconds of a token issue, the client is notified to retry after a short time, by which the updated value is expected to be set in the cache. 
4. In situations where returning stale data is not much problem, the client does not have to wait and stale data (at most 10 second old) is returned.
5. The response to the first request updates the Memcache
6. The pending and newer requests get fresh data from Memcache

# References:

1. [Memcached Architecture - Crash Course with Docker, Telnet, NodeJS](https://www.youtube.com/watch?v=NCePGsRZFus&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=100) (video)
2. [Introduction to Memcached](http://www.slideshare.net/oemebamo/introduction-to-memcached)
3. [Memcached Interview Questions from Javapoint](https://www.javatpoint.com/memcached-interview-questions-and-answers)
4. [Memcached Interview Questions from Wisdomjobs](https://www.wisdomjobs.com/e-university/memcached-interview-questions.html)
Most distributed systems receive multiple concurrent requests and need to perform concurrency control to prevent data inconsistencies because of interference between these requests.

One of these concurrency control methods is **locking**. However using locking in the context of a distributed system comes with a lot of edge-cases that add a lot of risks.

Of course, _distributed locking_ can also be modeled as a _consensus problem_, where the nodes of the system agree on a single value, which is the node that holds the lock.

# References:

1. [Distributed locks:](https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html)
2. [What is Distributed Locks??](https://mittal26081999.medium.com/distributed-locks-56031951a9e4)
3. [How to do distributed locking](https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html)
4. [Distributed Locks](https://ignite.apache.org/docs/latest/distributed-locks)
5. [Distributed Locking with Redis](https://varunkruthiventi.medium.com/distributed-locking-with-redis-af4e7d80b503)
6. [Chubby: Lock Service for Loosely Coupled Distributed Systems at Google](https://blog.acolyer.org/2015/02/13/the-chubby-lock-service-for-loosely-coupled-distributed-systems/)
7. [Distributed Locking at Uber](https://www.youtube.com/watch?v=MDuagr729aU)
8. [Distributed Locks using Redis at GoSquared](https://engineering.gosquared.com/distributed-locks-using-redis)
9. [ZooKeeper at Twitter](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2018/zookeeper-at-twitter.html)
10. [Eliminating Duplicate Queries using Distributed Locking at Chartio](https://blog.chartio.com/posts/eliminating-duplicate-queries-using-distributed-locking)
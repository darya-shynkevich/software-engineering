Caching ***improves application performance*** by ***temporarily storing frequently accessed data in memory, reducing the need to fetch it from slower storage systems***. However, when the original data changes, the cache must be updated or invalidated to maintain data consistency.

_**[Cache](https://github.com/donnemartin/system-design-primer#cache)** improves page load times and can reduce the load on your servers and databases. In this model, the dispatcher will first lookup if the request has been made before and try to find the previous result to return, in order to save the actual execution._

_Databases often benefit from a uniform distribution of reads and writes across its partitions. Popular items can skew the distribution, causing bottlenecks. Putting a cache in front of a database can help absorb uneven loads and spikes in traffic._

![Pasted image 20230826021400](../../../../../_Attachments/Pasted%20image%2020230826021400.png)

- **[Client-side caching](Client-side%20caching.md)** — *The client stores frequently accessed files locally. When a file is requested, the client checks if it has an up-to-date copy and uses it if available, reducing the need to request the file from the server.*
- **Server-side caching** — *The server stores frequently accessed files in memory or on local disks. When a file is requested, the server checks if it’s in the cache and returns it without accessing the disk, reducing disk I/O.*
- **[Distributed caching](Distributed%20caching.md)** — *This involves distributing the cache across multiple servers or nodes. When a file is requested, the system checks if it’s in the cache and returns it from the nearest server, minimizing network traffic.*
- [CPU level cache](CPU%20level%20cache.md) - *it works completely differently from the lock. and this is the reason why volatile is safe to be used in boolean type, it can guarantee memory visibility, but not automatic operation (still needs lock).*
  
  [Distributed caching](Distributed%20caching.md) is becoming more common due to the decreasing cost of main memory and the increasing speed of network cards. However, cache consistency remains a challenge. Ensuring that the cached data is coherent with the source of truth (e.g., the database) is essential to prevent inconsistencies in application behavior. Different schemes such as “cache-aside” and “read-through cache” are used to manage cache invalidation and consistency.

1. [Cache Invalidation](Cache%20Invalidation)
2. [Memcached](Memcached.md)
3. [Redis](Redis.md)


_[Hazelcast Python Client](https://github.com/hazelcast/hazelcast-python-client) —- is an open-source distributed in-memory data store and computation platform that provides a wide variety of distributed data structures and concurrency primitives._

[*](https://github.com/Yiling-J/theine)[https://github.com/Yiling-J/theine](https://github.com/Yiling-J/theine) — High performance in-memory cache inspired by [Caffeine](https://github.com/ben-manes/caffeine)*

# References:

1. [System Design — Caching](https://interviewnoodle.com/system-design-caching-498a0253cbff)
2. [The Fundamental Knowledge of System Design — (5) — Cache](https://interviewnoodle.com/the-fundamental-knowledge-of-system-design-5-b69bd2942917)
3. [Multicore Memory Caching Issues — Cache Coherence](https://interviewnoodle.com/multicore-memory-caching-issues-cache-coherence-60b1042f3713)
4. [Cache Problems: Cache Penetration, Cache Breakdown, Cache Avalanche](https://interviewnoodle.com/cache-problems-cache-penetration-cache-breakdown-cache-avalanche-9b866483e2b7)
5. [Caching — System Design Concept For Beginners](https://medium.com/@anuupadhyay1994/caching-system-design-concept-for-beginners-3fbe4874253d)
6. [Caching in System Design: An In-Depth Exploration](https://medium.com/@abhishekranjandev/caching-in-system-design-an-in-depth-exploration-b51e2c2e4dbd)
7. [System Design Interview Prep: Everything you need to know about Caching](https://bootcamp.uxdesign.cc/system-design-interview-prep-everything-you-need-to-know-about-caching-11b1529763e4)
8. [[По полочкам] Кэширование](https://habr.com/ru/articles/734660/)
9. [Deep Dive In Caching](https://vishalrana9915.medium.com/deep-dive-in-caching-9780bc55ea7)
10. **[Mastering the Art of Caching for System Design Interviews: A Complete Guide](https://levelup.gitconnected.com/master-the-art-of-caching-for-system-design-interviews-a-complete-guide-676bb49d194)**
11. [Basic Caching Techniques Explained - Spatial, Temporal, Distributed, Write-Through, Write-Back,Aside](https://www.youtube.com/watch?v=ccemOqDrc2I&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=51) (video)
12. [Caching things every Programmer must know](https://medium.com/javarevisited/caching-things-every-programmer-must-know-28d4a7e8b9b1)
13. [All things caching- use cases, benefits, strategies, choosing a caching technology, exploring some popular products](https://medium.datadriveninvestor.com/all-things-caching-use-cases-benefits-strategies-choosing-a-caching-technology-exploring-fa6c1f2e93aa)
14. [Caching in Python Using the LRU Cache Strategy](https://realpython.com/lru-cache-python/)
15. [BFCache, или Туда и обратно. Доклад Яндекса](https://habr.com/ru/company/yandex/blog/496360/)
17. [Данил Ахтаров. Кеширование — делаем всё правильно](https://www.youtube.com/watch?v=L0xmgTW3QAo) (video)
18. [10 Program Busting Caching Mistakes](http://highscalability.com/blog/2014/7/16/10-program-busting-caching-mistakes.html)
19. [Design Of A Modern Cache](http://highscalability.com/blog/2016/1/25/design-of-a-modern-cache.html)
20. [A Hitchhiker’s Guide to Caching Patterns](https://hazelcast.com/blog/a-hitchhikers-guide-to-caching-patterns/)
21. [Caching Guidance](https://learn.microsoft.com/en-us/previous-versions/msp-n-p/dn589802(v%3dpandp.10))
22. [Cache is King](https://www.stevesouders.com/blog/2012/10/11/cache-is-king/)
23. [Anti-Caching](https://www.the-paper-trail.org/post/2014-06-06-paper-notes-anti-caching/)
24. **[Don’t Forget to Discuss These 12 Aspects About Caching in System Design Interview](https://levelup.gitconnected.com/dont-forget-to-discuss-these-12-aspects-about-caching-in-system-design-interview-84d139885b9a)**
25. [**6-Caching Strategies to Remember while designing Cache System**](https://javascript.plainenglish.io/6-caching-strategies-to-remember-while-designing-cache-system-da058a3757cf)
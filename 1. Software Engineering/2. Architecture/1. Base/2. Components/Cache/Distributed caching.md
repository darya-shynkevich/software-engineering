A **distributed cache** is a caching system where multiple cache servers coordinate to store frequently accessed data. Distributed caches are needed in environments where a single cache server isn’t enough to store all the data. At the same time, it’s scalable and guarantees a higher degree of availability.

(+) minimize user-perceived latency by precalculating results and storing frequently accessed data.
(+) pre-generate expensive queries from the database.
(+) store user session data temporarily.
(+) serve data from temporary storage even if the data store is down temporarily.
(+) reduce network costs by serving data from local resources.

# Why distributed cache?

When the size of data required in the cache increases, storing the entire data in one system is impractical:
- It can be a potential single point of failure (SPOF).
- A system is designed in layers, and each layer should have its caching mechanism to ensure the decoupling of sensitive data from different layers.
- Caching at different locations helps reduce the serving latency at that layer.



# Cache Invalidation Strategies

*Cache invalidation is the process of removing or updating outdated data from a cache to ensure that only the most recent and accurate information is stored.*

## Write-through cache

Under this scheme, ***data is written into the cache and the corresponding database simultaneously***. The cached data allows for fast retrieval and, since the same data gets written in the permanent storage, we will have ***==complete data consistency== between the cache and the storage***. Also, this scheme ensures that nothing will get lost in case of a crash, power failure, or other system disruptions. Although, write-through ==***minimizes the risk of data loss***==, since every write operation must be done twice before returning success to the client, this scheme has the disadvantage of ***==higher latency for write operations**==* ***==+ a lot of space==***

**Example:** An e-commerce website updates its product inventory in real-time. Whenever a product's stock changes, the cache is also updated to reflect the new inventory count.

However the data loss can occur: [Cache Invalidation - Data loss solution](Cache%20Invalidation%20-%20Data%20loss%20solution.md)

![Pasted image 20231014201619](../../../../../_Attachments/Pasted%20image%2020231014201619.png)

## Write-around cache

This technique is similar to write-through cache, but ***data is written directly to permanent storage, bypassing the cache***. This can ***==reduce the cache being flooded==*** with write operations that will not subsequently be re-read, but has the disadvantage that a read request for recently written data will create a ***==“cache miss”==*** and must be read from slower back-end storage and experience higher latency.

Update cache in the background???

**Example:** An application updates user profile information, which is infrequently accessed. The application writes the new data directly to the data store, avoiding unnecessary cache updates.

However the data loss can occur: [Cache Invalidation - Data loss solution](Cache%20Invalidation%20-%20Data%20loss%20solution.md)

![Pasted image 20231014201715](../../../../../_Attachments/Pasted%20image%2020231014201715.png)

## Write-back cache (or lazy-write)

Under this scheme, ***data is written to cache alone***, and completion is immediately confirmed to the client. ***The write to the permanent storage is done*** based on certain conditions, for example, ***when the system needs some free space***. This results in *==low-latency and high-throughput for write-intensive applications; however, this speed comes with the risk of data loss in case of a crash or other adverse event because the only copy of the written data is in the cache.==*

**Example:** Imagine a collaborative document editing application that allows multiple users to make changes to a document simultaneously. When users make changes, those changes are first saved to the cache, allowing the application to respond quickly and provide a smooth editing experience. When certain conditions are met (e.g., the number of changes reaches a certain threshold), the application writes the cached changes back to the data store, updating the document with the latest changes from all users. This approach minimizes the number of write operations to the data store and reduces the load on the storage system, improving the overall performance of the application. (Google Documents)

## Write-behind cache

It is quite similar to write-back cache. In this scheme, data is written to the cache and acknowledged to the application immediately, but it is not immediately written to the permanent storage. Instead, the write operation is deferred, and the data is eventually written to the permanent storage at a later time. ***The main difference between write-back cache and write-behind cache is ==when== the data is written to the permanent storage.*** In write-back caching, data is only written to the permanent storage when it is necessary for the cache to free up space or when an event happens, while in ***==write-behind caching, data is written to the permanent storage at specified intervals==***.

**Example:** A document editor application temporarily saves changes to the cache while the user is editing. Periodically, the changes are written back to the data store to minimize the number of write operations. (Google Documents)

![Pasted image 20231014172227](../../../../../_Attachments/Pasted%20image%2020231014172227.png)


# References:

1. [**Distributed Caching**](https://www.wix.engineering/post/scaling-to-100m-to-cache-or-not-to-cache)
2. [What is Distributed Caching? Explained with Redis!](https://www.youtube.com/watch?v=U3RkDLtS7uY&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=11)
3. [System Design Interview - Distributed Cache](https://www.youtube.com/watch?v=iuqZvajTOyA) (video)
4. [A Comprehensive Guide to Distributed Caching](https://medium.com/bytebytego-system-design-alliance/a-comprehensive-guide-to-distributed-caching-827f1fa5a184)
5. [Redis system design | Distributed cache System design](https://www.youtube.com/watch?v=DUbEgNw-F9c&list=PLkQkbY7JNJuBoTemzQfjym0sqbOHt5fnV&index=10) (video)
6. [EVCache: Distributed In-memory Caching at Netflix](https://medium.com/netflix-techblog/caching-for-a-global-netflix-7bcc457012f1)
7. [EVCache Cache Warmer Infrastructure at Netflix](https://medium.com/netflix-techblog/cache-warming-agility-for-a-stateful-service-2d3b1da82642)
8. [Memsniff: Robust Memcache Traffic Analyzer at Box](https://blog.box.com/blog/introducing-memsniff-robust-memcache-traffic-analyzer/)
9. [Caching with Consistent Hashing and Cache Smearing at Etsy](https://codeascraft.com/2017/11/30/how-etsy-caches/)
10. [Analysis of Photo Caching at Facebook](https://code.facebook.com/posts/220956754772273/an-analysis-of-facebook-photo-caching/)
11. [Cache Efficiency Exercise at Facebook](https://code.facebook.com/posts/964122680272229/web-performance-cache-efficiency-exercise/)
12. [tCache: Scalable Data-aware Java Caching at Trivago](http://tech.trivago.com/2015/10/15/tcache/)
13. [Pycache: In-process Caching at Quora](https://engineering.quora.com/Pycache-lightning-fast-in-process-caching)
14. [Reduce Memcached Memory Usage by 50% at Trivago](http://tech.trivago.com/2017/12/19/how-trivago-reduced-memcached-memory-usage-by-50/)
15. [Caching Internal Service Calls at Yelp](https://engineeringblog.yelp.com/2018/03/caching-internal-service-calls-at-yelp.html)
16. [Estimating the Cache Efficiency using Big Data at Allegro](https://allegro.tech/2017/01/estimating-the-cache-efficiency-using-big-data.html)
17. [Distributed Cache at Zalando](https://jobs.zalando.com/tech/blog/distributed-cache-akka-kubernetes/)
18. [Application Data Caching from RAM to SSD at NetFlix](https://medium.com/netflix-techblog/evolution-of-application-data-caching-from-ram-to-ssd-a33d6fa7a690)
19. [Tradeoffs of Replicated Cache at Skyscanner](https://medium.com/@SkyscannerEng/the-tradeoffs-of-a-replicated-cache-b6680c722f58)
20. [Avoiding Cache Stampede at DoorDash](https://blog.doordash.com/avoiding-cache-stampede-at-doordash-55bbf596d94b)
21. [Location Caching with Quadtrees at Yext](http://engblog.yext.com/post/geolocation-caching)
22. [Video Metadata Caching at Vimeo](https://medium.com/vimeo-engineering-blog/video-metadata-caching-at-vimeo-a54b25f0b304)
23. [Scaling Redis at Twitter](http://highscalability.com/blog/2014/9/8/how-twitter-uses-redis-to-scale-105tb-ram-39mm-qps-10000-ins.html)
24. [Scaling Job Queue with Redis at Slack](https://slack.engineering/scaling-slacks-job-queue-687222e9d100)
25. [Moving persistent data out of Redis at Github](https://githubengineering.com/moving-persistent-data-out-of-redis/)
26. [Storing Hundreds of Millions of Simple Key-Value Pairs in Redis at Instagram](https://engineering.instagram.com/storing-hundreds-of-millions-of-simple-key-value-pairs-in-redis-1091ae80f74c)
27. [Redis at Trivago](http://tech.trivago.com/2017/01/25/learn-redis-the-hard-way-in-production/)
28. [Optimizing Redis Storage at Deliveroo](https://deliveroo.engineering/2017/01/19/optimising-membership-queries.html)
29. [Memory Optimization in Redis at Wattpad](http://engineering.wattpad.com/post/23244724794/store-more-stuff-memory-optimization-in-redis)
30. [Redis Fleet at Heroku](https://blog.heroku.com/rolling-redis-fleet)
31. [Solving Remote Build Cache Misses (2 parts) at SoundCloud](https://developers.soundcloud.com/blog/gradle-remote-build-cache-misses-part-2)
32. [Ratings & Reviews (2 parts) at Flipkart](https://blog.flipkart.tech/ratings-reviews-flipkart-part-2-574ab08e75cf)
33. [Prefetch Caching of Items at eBay](https://tech.ebayinc.com/engineering/prefetch-caching-of-ebay-items/)
34. [Cross-Region Caching Library at Wix](https://www.wix.engineering/post/how-we-built-a-cross-region-caching-library)
35. [Improving Distributed Caching Performance and Efficiency at Pinterest](https://medium.com/pinterest-engineering/improving-distributed-caching-performance-and-efficiency-at-pinterest-92484b5fe39b)


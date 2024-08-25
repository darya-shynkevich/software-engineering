
### Instagram

Instagram uses [Cassandra](Cassandra.md) to support the news user feed. They used geo replication (the Cassandra cluster in Europe stores the data of European users. While the Cassandra cluster in the US stores the data of the US users.)

Yet there’s a [risk of downtime](The%20Real%20Cost%20of%20Slow%20Time%20vs%20Downtime.md) with separate Cassandra clusters for each region => so they keep a single replica in another region and use [Quorum](Quorum.md)) to route requests with an extra latency

Besides they use [Akkio](https://engineering.fb.com/2018/10/08/core-infra/akkio/) for optimal data placement. It splits data into logical units with strong locality (imagine **Akkio** as a service that knows when and how to move data for low latency)

They route every user request through the global load balancer. And here’s how they find the right Cassandra cluster for a request:
![](Pasted%20image%2020240623174337.png)
1. The user sends a request to the app but it doesn’t know the user's Cassandra cluster
2. The app forwards the request to the Akkio proxy
3. The Akkio proxy asks the cache layer
4. The Akkio proxy will talk to the location database on a cache miss. It’s replicated across every region as it has a small dataset
5. The Akkio proxy returns the user's Cassandra cluster information to the app
6. The app directly accesses the right Cassandra cluster based on the response
7. The app caches the information for future requests

The Akkio requests take around 10 ms. So the extra latency is tolerable.

Also they migrate the user data if a user moves to another continent and settles down there.

Besides they store user information, friendships, and picture metadata in [PostgreSQL](https://www.postgresql.org/). Yet they get more read requests than write requests => so they run PostgreSQL in [leader-follower](Single%20leader.md) replication topology. Write requests go to the leader. While read requests get routed to the follower on the same data center.

Besides they use [Memcache](https://memcached.org/) to protect PostgreSQL against massive traffic. And route read requests to it. No Memcache replication across data centers for _low latency_.
For example, comments received on a picture in one data center wouldn’t be visible to someone viewing the same picture from another data center.
![](Pasted%20image%2020240623184601.png)

In Instagram they use [denormalized tables](https://www.splunk.com/en_us/blog/learn/data-denormalization.html) to handle expensive queries => faster responses with less resource usage.

But invalidating every cache at once could cause a [thundering herd problem](https://en.wikipedia.org/wiki/Thundering_herd_problem) on PostgreSQL.

So they use [**Memcache lease**](Memcached.md).

# References:

1. ~~[How Instagram Scaled to 2.5 Billion Users](https://newsletter.systemdesign.one/p/instagram-infrastructure?utm_source=substack&publication_id=1511845&post_id=145237784&utm_medium=email&utm_content=share&utm_campaign=email-share&triggerShare=true&isFreemail=true&r=1vxw4z&triedRedirect=true)~~
2. [High-Level Design for Instagram News Feed](https://medium.com/@interviewready/high-level-design-for-instagram-news-feed-423773d790f8)
3. [Designing Instagram: System Design of News Feed](https://www.youtube.com/watch?v=QmX2NPkJTKg&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=23) (video)
4. [Deisgn a News Feed System](https://medium.com/@bansal_ankur/design-a-news-feed-system-6bf42e9f03fb)
5. [What are best practices for building something like a News Feed?](http://www.quora.com/What-are-best-practices-for-building-something-like-a-News-Feed)
6. [What are the scaling issues to keep in mind while developing a social network feed?](http://www.quora.com/Activity-Streams/What-are-the-scaling-issues-to-keep-in-mind-while-developing-a-social-network-feed)
7. [Activity Feeds Architecture](http://www.slideshare.net/danmckinley/etsy-activity-feeds-architecture)
8. [Architecture of Ad Platform at Twitter](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2020/building-twitters-ad-platform-architecture-for-the-future.html)
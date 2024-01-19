![Pasted image 20230605121444](../../../../../../_Attachments/Pasted%20image%2020230605121444.png)

Sharding is a process of segmenting the data into partitions that are spread on multiple DB instances => *==it is a method for distributing data across multiple machines==*. It becomes especially handy when no single machine can handle the expected workload.

*Sharding is an example of horizontal scaling, while vertical scaling is an example of just getting larger and larger machines to support the new workload.*
![Pasted image 20230605121659](../../../../../../_Attachments/Pasted%20image%2020230605121659.png)
*==A sharded database may be the best way to go if your application database manages a lot of data, needs a lot of reads and writes, and/or needs to be available at all times.==*

*==**Sharding allows for significant scalability, but comes with increased complexity, overhead, and infrastructure costs.***==
## [What are my options before?](What%20are%20my%20options%20before?.md)

## Pros of Sharding

(+) Scalability (horizontal scalability)
	Data
	Memory
(+) Security (users can access certain shards)
(+) Optimal and smaller index size
## Cons of Sharding

(-) Complex client (aware of the shard)
(-) Transactions across shards problem. 
	Almost impossible to do atomic multi-shard transaction
(-) Rollbacks
(-) Schema changes are hard
(-) JOINS across DBs
(-) Has to be something you know in the query
## [Consistent Hashing](../../../../../2.%20Architecture/1.%20Base/1.%20Concepts/Consistent%20Hashing.md)

## How does it work?

1. How do we distribute the data across shards? Are there potential [Hotspot](../../../../../2.%20Architecture/1.%20Base/1.%20Concepts/Hotspot.md)s if data isn't distributed evenly?
2. What queries do we run and how do the tables interact?
3. How will data grow? How will it need to be redistributed later?

**Shard Key** is a piece of the **primary key** that tells how the data should be distributed. With a shard key, you can quickly find and change data by routing operations to the correct database.

The same node houses entries that have the same shard key. A group of data that shares the same shard key is referred to as a **logical shard**. Multiple logical shards are included in a database node, also known as a **physical shard**.
## [Cross Shard Transactions](Cross%20Shard%20Transactions.md)

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
16. [Troubles With Sharding - What Can We Learn From The Foursquare Incident?](http://highscalability.com/blog/2010/10/15/troubles-with-sharding-what-can-we-learn-from-the-foursquare.html)
17. [The Dark Side of Sharding: Mistakes You Can’t Afford to Make](https://levelup.gitconnected.com/the-dark-side-of-sharding-mistakes-you-cant-afford-to-make-7905a6c45499)
18. [Sharding The Hibernate Way](http://highscalability.com/blog/2008/7/26/sharding-the-hibernate-way.html)
19. [Sharding MySQL at Pinterest](https://medium.com/@Pinterest_Engineering/sharding-pinterest-how-we-scaled-our-mysql-fleet-3f341e96ca6f)
20. [Sharding MySQL at Twilio](https://www.twilio.com/engineering/2014/06/26/how-we-replaced-our-data-pipeline-with-zero-downtime)
21. [Sharding MySQL at Square](https://medium.com/square-corner-blog/sharding-cash-10280fa3ef3b)
22. [Sharding MySQL at Quora](https://www.quora.com/q/quoraengineering/MySQL-sharding-at-Quora)
23. [Sharding Layer of Schemaless Datastore at Uber](https://eng.uber.com/schemaless-rewrite/)
24. [Sharding & IDs at Instagram](https://instagram-engineering.com/sharding-ids-at-instagram-1cf5a71e5a5c)
25. [Sharding Postgres at Notion](https://www.notion.so/blog/sharding-postgres-at-notion)
26. [Solr: Improving Performance for Batch Indexing at Box](https://blog.box.com/blog/solr-improving-performance-batch-indexing/)
27. [Geosharded Recommendations (3 parts) at Tinder](https://medium.com/tinder-engineering/geosharded-recommendations-part-3-consistency-2d2cb2f0594b)
28. [Scaling Services with Shard Manager at Facebook](https://engineering.fb.com/production-engineering/scaling-services-with-shard-manager/)
*==Database replication is a strategy employed to maintain multiple copies of a single database across different servers or locations, with the aim of enhancing data availability, redundancy, and fault tolerance.==*

In a typical database replication setup, there exists a primary (or master) database along with one or more replica (or slave) databases. The primary goal is to ensure the ***consistency of data between the primary and replica databases by synchronising them***. This synchronisation process involves updating the replica databases whenever changes are applied to the primary database. ***The primary database serves as the definitive source of data, while the replica databases mirror its content.***

(+) **Scalability:** leads to ***improved system performance*** by distributing read queries across multiple replica databases. This reduces the workload on the primary database, resulting in faster query response times and overall system optimisation + enables load balancing by ***directing read queries to replica databases***, distributing query processing, and enhancing system performance + you can put replicas in different locations => ***enhance the speed of data retrieval*** (users in Australia will retrieve data from Sydney faster, than from New York)
(+) **Data Availability:** ensures ***high availability***, meaning that if the primary database encounters downtime or failure, the replica databases can seamlessly take over, allowing users to access data without disruption. 
(+) **Fault tolerance:** provides an ***added layer of data protection*** ***by maintaining redundant copies*** of the database in different locations, safeguarding against data loss due to hardware failures or unforeseen events. 
# Implementation of Replication Logs

 1. **Statement based replication:** every sql query is sending from master to primary DB; Naive implementation because SQL server will have to do optimisations again + problems with `NOW()`. 
	 ! This type of replication was used in MySQL before version 5.1.
 2. **WAL shipping** -> the primary node saves the query before executing it in a log file known as a write-ahead log file. It then uses these logs to copy the data onto the secondary nodes. This is used in PostgreSQL and Oracle. The problem with WAL is that it *only defines data at a very low level*. It’s tightly coupled with the inner structure of the database engine, which makes upgrading software on the leader and followers complicated.
 2. **Streaming replication:** no need to wait for commit, can stream during the write to the master
 3. **Logical Replication:** does not stream the binary data from WAL because the binary representation of things changes from one release to another. So Postgres do Logical Replication (a higher level abstraction) and send them to the replica. + A logical log format is also easier for external applications to parse. This aspect is useful if you want to send the contents of a database to an external system, such as a [[Data Warehouse]] for offline analysis, or for building custom indexes and caches
	 ! Postgres 14 have Streaming Logical Replication
4. **Trigger-based replication:** typically has greater overheads than other replication methods, and is more prone to bugs and limitations than the database’s built-in replication. However, it can nevertheless be useful due to its flexibility.
# [[Data Replication Strategies]]

Replication can be synchronous or asynchronous, which has a profound effect on the system behavior when there is a fault. Although asynchronous replication can be fast when the system is running smoothly, it’s important to figure out what happens when replication lag increases and servers fail. If a leader fails and you promote an asynchronously updated follower to be the new leader, recently committed data may be lost.
# Data Replication Models

## 1. [Single leader / primary-secondary replication](Models/Single%20leader.md)

Clients send all writes to a single node (the leader), which sends a stream of data change events to the other replicas (followers). Reads can be performed on any replica, but reads from followers might be stale.
## 2. [Multi-leader replication](Models/Multi%20leader.md)

Clients send each write to one of several leader nodes, any of which can accept writes. The leaders send streams of data change events to each other and to any follower nodes.
## 3. [Peer-to-peer / leaderless replication](Models/Leaderless%20replication.md)

Clients send each write to several nodes, and read from several nodes in parallel in order to detect and correct nodes with stale data.

# Challenges in Database Replication

## 1. Data Conflicts

The Scenario: Multiple replicas receiving different updates can lead to discrepancies in data.

Diving Deeper: If two users update the same piece of data simultaneously in different replicas, a conflict arises. Which update should be considered valid? Resolving such conflicts, especially in asynchronous replication, demands robust conflict resolution strategies.
## 2. Network Overhead

The Scenario: Continuous communication between the primary and replica databases can strain network resources.

Diving Deeper: Particularly in synchronous replication, every data update needs acknowledgment from replicas. This continuous chatter can lead to bandwidth saturation, potentially slowing down other network operations.
## 3. Latency Issues

The Scenario: The time taken for data to travel and get updated across databases can introduce delays.

Diving Deeper: In geographically dispersed databases, the physical distance can cause latency, resulting in delays in reflecting updates. This can be especially challenging in applications demanding real-time data consistency.
## 4. Maintenance Complexity

The Scenario: More replicas can mean more points of potential failures and maintenance overheads.

Diving Deeper: Replicas, while providing redundancy, also introduce complexity. Each replica might need patches, updates, or backups. Ensuring all replicas are uniformly maintained can be a resource-intensive task.
## 5. Cost Implications

The Scenario: Implementing and running multiple replicas can escalate costs.

Diving Deeper: Infrastructure costs for hosting replicas, network costs for data transfer, and human resource costs for maintenance can add up. It’s vital to strike a balance between the benefits of replication and its financial implications.
# [Future Trends](Future%20Trends.md)

# References:

1. [Replication in Distributed Systems - Part 1](https://manthanguptaa.in/posts/replication_in_distributed_systems_part_1/)
2. ~~[Mastering Database Replication: An Essential Guide for 2023](https://levelup.gitconnected.com/mastering-database-replication-an-essential-guide-for-2023-9fa6deb3efe4)~~
3. [~~All Types of Database Replication Discussed](https://www.youtube.com/watch?v=aE2UPg3Ckck&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=75) (video)~~
4. [Database Replication Crash Course ( with Postgres 13 )](https://www.youtube.com/watch?v=9aFu7APZQmY&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=14) (video)
5. [Data Replication Strategies I Wish I Knew Before the System Design Interview](https://levelup.gitconnected.com/i-wished-i-knew-these-data-replication-strategies-before-the-system-design-interview-e228216290f3)
6. [The Inner Workings of Distributed Databases](https://questdb.io/blog/inner-workings-distributed-databases/)
7. [Борьба с нагрузкой в PostgreSQL, помогает ли репликация в этом?](https://www.youtube.com/watch?v=xY_M8TGdm6A&list=PLH-XmS0lSi_zgalbXwsytGNdAlNYmmE5C&index=15) (video)
8. [Асинхронная репликация без цензуры](https://highload.guide/blog/asynchronous-replication.html)
9. [Lazy Replication: Exploiting the Semantics of Distributed Services](https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.17.469)
10. [Optimistic Replication](https://www.hpl.hp.com/techreports/2002/HPL-2002-33.pdf) - Relaxed consistency approaches for data replication
11. [Репликация между разными СУБД](https://www.youtube.com/watch?v=6rWhswb_3YA&list=PLH-XmS0lSi_wRIh4RJjnTGMKaTiQoaGTc&index=88) (video)
12. [Black-Box Auditing: Verifying End-to-End Replication Integrity between MySQL and Redshift at Yelp](https://engineeringblog.yelp.com/2018/04/black-box-auditing.html)
13. [Read Consistency with Database Replicas at Shopify](https://shopify.engineering/read-consistency-database-replicas)
14. [Netflix Data replication - Change Data Capture](https://netflixtechblog.com/dblog-a-generic-change-data-capture-framework-69351fb9099b)
15. [Herb: Multi-DC Replication Engine for Schemaless Datastore at Uber](https://eng.uber.com/herb-datacenter-replication/)

## MySQL

1. ! [Как устроена MySQL-репликация](https://highload.guide/blog/mysql-replication.html)
2. [Demystifying MySQL Replication Crash Safety](https://www.youtube.com/watch?v=F2mZYTDsNDk&list=PLH-XmS0lSi_wRIh4RJjnTGMKaTiQoaGTc&index=111) (video)
3. [Turns out MySQL Statement-based Replication might not be a good idea, Lets discuss why](https://www.youtube.com/watch?v=WSDjjgSP-jA&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=80) (video)
4. [MySQL Parallel Replication (4 parts) at Booking.com](https://medium.com/booking-com-infrastructure/evaluating-mysql-parallel-replication-part-4-annex-under-the-hood-eb456cf8b2fb)
5. [Mitigating MySQL Replication Lag and Reducing Read Load at Github](https://githubengineering.com/mitigating-replication-lag-and-reducing-read-load-with-freno/)
6. [Раскрытие секретов Group Replication в MySQL](https://www.youtube.com/watch?v=40nYX-ZIdvw&list=PLH-XmS0lSi_zTZrols83QSxI3Q96dSbBm&index=16) (video)

## PostgreSQL

1. [Выбираем систему репликации для PostgreSQL](https://www.youtube.com/watch?v=YQ63niptCTc&list=PLH-XmS0lSi_wRIh4RJjnTGMKaTiQoaGTc&index=50) (video)
2. [Масштабирование реплик PostgreSQL под нагрузкой](https://www.youtube.com/watch?v=IyIdf-AEz5U&list=PLH-XmS0lSi_wRIh4RJjnTGMKaTiQoaGTc&index=41) (video)
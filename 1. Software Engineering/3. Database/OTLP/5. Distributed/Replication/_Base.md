![Pasted image 20230826022023](../../../../../_Attachments/Pasted%20image%2020230826022023.png)

*==Database replication is a strategy employed to maintain multiple copies of a single database across different servers or locations, with the aim of enhancing data availability, redundancy, and fault tolerance.==*

In a typical database replication setup, there exists a primary (or master) database along with one or more replica (or slave) databases. The primary goal is to ensure the ***consistency of data between the primary and replica databases by synchronizing them***. This synchronization process involves updating the replica databases whenever changes are applied to the primary database. ***The primary database serves as the definitive source of data, while the replica databases mirror its content.***

### Advantages:

1. Scalability: leads to ***improved system performance*** by distributing read queries across multiple replica databases. This reduces the workload on the primary database, resulting in faster query response times and overall system optimization + enables load balancing by ***directing read queries to replica databases***, distributing query processing, and enhancing system performance + you can put replicas in different locations => ***enhance the speed of data retrieval*** (users in Australia will retrieve data from Sydney faster, than from New York)
2. Data Availability: ensures ***high availability***, meaning that if the primary database encounters downtime or failure, the replica databases can seamlessly take over, allowing users to access data without disruption. 
3. Fault tolerance: provides an ***added layer of data protection*** ***by maintaining redundant copies*** of the database in different locations, safeguarding against data loss due to hardware failures or unforeseen events. 

#### Database Replication Strategies

##### Synchronous Replication

Synchronous replication is a type of database replication where ***changes made to the primary database are immediately replicated*** to the replica databases ***before the write operation is considered complete***. In other words, the primary database waits for the replica databases to confirm that they have received and processed the changes before the write operation is acknowledged.

![Pasted image 20231014145242](../../../../../_Attachments/Pasted%20image%2020231014145242.png)

**Pros:**
- Consistency: Since replicas update instantly, all databases remain in harmony, eliminating data mismatches.
- Immediate Feedback: Any issue during data transfer is instantly known, allowing swift corrections.

**Cons:**
- Latency Issues: Waiting for confirmation from the replica can introduce delays, affecting performance.
- Dependency: If a replica is unreachable, the whole update process can stall.

**Ideal For**: Systems where data integrity and consistency are paramount, such as financial and medical databases.

##### Asynchronous Replication

Asynchronous replication is a type of database replication where ***changes made to the primary database are not immediately replicated to the replica databases***. Instead, the ***changes are queued and replicated to the replicas at a later time***.

![Pasted image 20231014145400](../../../../../_Attachments/Pasted%20image%2020231014145400.png)

**Pros:**
- Performance Boost: Without waiting for confirmations, operations are faster.
- Resilience: Even if a replica is down, the primary continues functioning without hitches.

**Cons:**
- Data Lag: Replicas might not always have the latest data immediately.
- Potential Data Loss: If the primary crashes before a replica updates, that recent data might be lost.

**Ideal For**: Web applications where speed is crucial and minor data discrepancies can be tolerated.

##### **Semi-synchronous Replication**

Semi-synchronous replication is a type of database replication that combines elements of both synchronous and asynchronous replication. In semi-synchronous replication, ***changes made to the primary database are immediately replicated to at least one replica database, while other replicas may be updated asynchronously.***

In semi-synchronous replication, the write operation on the primary is not considered complete until ***at least one replica database has confirmed that it has received and processed the changes.*** This ensures that there is some level of strong consistency between the primary and replica databases, while also providing improved performance compared to fully synchronous replication.

![Pasted image 20231014145528](../../../../../_Attachments/Pasted%20image%2020231014145528.png)

**Pros:**
- Balanced Approach: It combines the speed of asynchronous with the reliability of synchronous replication.
- Guaranteed Backup: At least one replica is always up-to-date, ensuring data safety.

**Cons:**
- Partial Delays: While faster than fully synchronous, waiting for at least one confirmation can introduce some lag.
- Complexity: Implementing and maintaining this hybrid can be more intricate.

**Ideal For**: E-commerce platforms where a balance of speed and data safety is necessary.

#### Factors to Consider When Choosing a Replication Strategy

##### **Data Consistency Requirements**

**What’s At Stake?**: How crucial is it for your replicas to mirror the primary database in real-time?

**Example**: In financial systems, where a slight inconsistency can lead to significant discrepancies, immediate consistency is non-negotiable. On the other hand, a blogging platform can afford a slight lag between the primary and replicas.

##### **Write Performance and Latency**

**What’s At Stake?**: Speed! Do you prioritize rapid write operations, or can you afford a slight delay for data integrity?

**Example**: A real-time gaming platform cannot afford latency in updates. Here, the speed of writing data (even if it compromises immediate consistency) can be more critical than in a digital library, where a minor delay in updating a catalog might be acceptable.

##### **==Network Infrastructure and Bandwidth==**

**What’s At Stake?**: The physical aspects of your system. Can your network handle constant communication between primary and replicas?

**Example**: *==In regions with unreliable or slow network connections, asynchronous replication might be more feasible than synchronous, which requires constant and speedy communication.==*

##### **Geographical Distribution of Databases**

**What’s At Stake?**: The locations of your databases and their distances from one another.

**Example**: If you have a globally dispersed user base, it might make sense to have replicas in various continents. However, *==the farther apart these databases are, the higher the latency, which might push you towards an asynchronous or semi-synchronous approach.==*

##### **Recovery and Failover Strategies**

**What’s At Stake?**: How quickly and smoothly can your system recover from failures?

**Example**: E-commerce platforms that see heavy traffic, especially during sales, need robust recovery strategies. If the primary server fails, a replica needs to take over swiftly to avoid revenue losses. Here, the strategy should align with quick failover capabilities.

#### Challenges in Database Replication

##### Data Conflicts

The Scenario: Multiple replicas receiving different updates can lead to discrepancies in data.

Diving Deeper: If two users update the same piece of data simultaneously in different replicas, a conflict arises. Which update should be considered valid? Resolving such conflicts, especially in asynchronous replication, demands robust conflict resolution strategies.

##### Network Overhead

The Scenario: Continuous communication between the primary and replica databases can strain network resources.

Diving Deeper: Particularly in synchronous replication, every data update needs acknowledgment from replicas. This continuous chatter can lead to bandwidth saturation, potentially slowing down other network operations.

##### Latency Issues

The Scenario: The time taken for data to travel and get updated across databases can introduce delays.

Diving Deeper: In geographically dispersed databases, the physical distance can cause latency, resulting in delays in reflecting updates. This can be especially challenging in applications demanding real-time data consistency.

##### Maintenance Complexity

The Scenario: More replicas can mean more points of potential failures and maintenance overheads.

Diving Deeper: Replicas, while providing redundancy, also introduce complexity. Each replica might need patches, updates, or backups. Ensuring all replicas are uniformly maintained can be a resource-intensive task.

##### Cost Implications

The Scenario: Implementing and running multiple replicas can escalate costs.

Diving Deeper: Infrastructure costs for hosting replicas, network costs for data transfer, and human resource costs for maintenance can add up. It’s vital to strike a balance between the benefits of replication and its financial implications.

#### Future Trends in Database Replication

##### Multi-Modal Replication
The Scenario: Combining various replication techniques tailored for specific tasks within the same system.

Diving Deeper: Instead of sticking to one replication type (e.g., synchronous), future systems might mix and match based on data criticality. Transactional data might use synchronous replication for accuracy, while less critical data might use asynchronous for speed.

##### AI-Powered Replication Management

##### Edge Replication
The Scenario: As edge computing grows, bringing data processing closer to data sources, replication strategies will adapt.

Diving Deeper: Replicas might not just be centralized in data centers but also span across edge devices, from IoT gadgets to local servers, ensuring faster data access and processing at the source.

##### Enhanced Security Protocols
The Scenario: As replication involves data transfer across networks, security becomes paramount.

Diving Deeper: Future replication tools will incorporate advanced encryption techniques, zero-trust architectures, and real-time threat detection to ensure data safety during transfers and at rest.

##### Autonomous Replication
The Scenario: Minimizing human intervention in replication processes.

Diving Deeper: With advancements in automation, we can expect replication processes that self-manage, from auto-detection of replication needs to self-healing from failures, reducing the manual overhead.

# References:

1. ~~[Mastering Database Replication: An Essential Guide for 2023](https://levelup.gitconnected.com/mastering-database-replication-an-essential-guide-for-2023-9fa6deb3efe4)~~
2. [All Types of Database Replication Discussed](https://www.youtube.com/watch?v=aE2UPg3Ckck&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=75) (video)
3. [Database Replication Crash Course ( with Postgres 13 )](https://www.youtube.com/watch?v=9aFu7APZQmY&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=14) (video)
4. [Data Replication Strategies I Wish I Knew Before the System Design Interview](https://levelup.gitconnected.com/i-wished-i-knew-these-data-replication-strategies-before-the-system-design-interview-e228216290f3)
5. [The Inner Workings of Distributed Databases](https://questdb.io/blog/inner-workings-distributed-databases/)
6. [Борьба с нагрузкой в PostgreSQL, помогает ли репликация в этом?](https://www.youtube.com/watch?v=xY_M8TGdm6A&list=PLH-XmS0lSi_zgalbXwsytGNdAlNYmmE5C&index=15) (video)
7. [Асинхронная репликация без цензуры](https://highload.guide/blog/asynchronous-replication.html)
8. [Lazy Replication: Exploiting the Semantics of Distributed Services](https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.17.469)
9. [Optimistic Replication](https://www.hpl.hp.com/techreports/2002/HPL-2002-33.pdf) - Relaxed consistency approaches for data replication
10. [Репликация между разными СУБД](https://www.youtube.com/watch?v=6rWhswb_3YA&list=PLH-XmS0lSi_wRIh4RJjnTGMKaTiQoaGTc&index=88) (video)
11. [Black-Box Auditing: Verifying End-to-End Replication Integrity between MySQL and Redshift at Yelp](https://engineeringblog.yelp.com/2018/04/black-box-auditing.html)
12. [Read Consistency with Database Replicas at Shopify](https://shopify.engineering/read-consistency-database-replicas)
13. [Netflix Data replication - Change Data Capture](https://netflixtechblog.com/dblog-a-generic-change-data-capture-framework-69351fb9099b)
14. [Herb: Multi-DC Replication Engine for Schemaless Datastore at Uber](https://eng.uber.com/herb-datacenter-replication/)

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
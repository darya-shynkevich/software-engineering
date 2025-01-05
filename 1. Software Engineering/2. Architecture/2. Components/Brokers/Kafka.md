Apache Kafka is designed to manage ***high-throughput***, ***real-time data streaming*** while maintaining ***fault tolerance***. It functions through a ***topic-based structure***, ***where publishers dispatch messages to specific topics and subscribers or consumers access these messages***. 

Kafka enables us the with three capabilities of a streaming platform:
1. **Publishing** (writing) and **subscribing to** (reading) streams of events
2. **Storing** streams of events durably and reliably
3. **Processing** streams of events as they occur

**Features:**
1. Distributed
2. Resilient & Fault Tolerant
3. Horizontally scalable
4. High performance & low latency
5. Schema agnostic storage
6. Polyglot (Java, Python, Go, and even REST Proxy)
7. Asynchronous processing & decoupling between producer & consumers
8. Persistent message store, buffer, bursty load, and consumer failures
9. Production tested, used by LinkedIn, Netflix, Airbnb, Walmart…

**Use cases of Kafka:**
1. Command Query Responsibility Segregation ([CQRS](CQRS.md))
2. Complex Event Processing (CEP)
3. Staged Event-Driven Architecture (SEDA)
4. Log Shipping & Aggregation
5. Event Sourcing
6. Change Data Capture
7. Scalable message store
8. Request-reply Integration pattern
9. Materialised View Pattern

# Kafka's components

## Producers

Producers are services/applications that generate messages/data streams. They connect to a Kafka Cluster using a **Broker** or a **Zookeeper** URL. **All events having the same key will be published in the same partition**, which is a way to guarantee the ordering when consuming events. This is done using key hashing by murmur2 algorithm.
### Message

A message produce by Producer will have following attributes:
1. Key — binary | Optional  (Business Data)
2. Value — binary | Optional  (Business Data)
3. Compression — none | gzip | snappy | zstd | lz4  
4. Headers — Metadata of message | Key: Value | Optional  Example: MIME-type, request_uuid, ...
5. Partition & Offset  
6. Timestamp

When communicating with a Kafka cluster, all messages are sent to the partition’s leader. The leader is responsible for writing the message to its own in sync replica and, once that message has been committed, is responsible for propagating the message to additional replicas on different brokers. Each replica acknowledges that they have received the message and can now be called in sync. 
### Writing strategy

![](../../../../../_Attachments/Pasted%20image%2020240121160514.png)

Example: card number can be a partition key => we guarantee that events about that card will go in write-in-partition order.

If there is no key => default partition strategy "round robin".
![](../../../../../_Attachments/Pasted%20image%2020240121160853.png)
Ack — It denotes the number of brokers that must receive the record before we consider the write as successful. Valid values are 0, 1, and ALL.

> 0 (none) → producer won’t wait for a response from the broker **(At most once)**
> 1 (leader) → producer will consider write successful when the leader receives the record **(At least once)**. *Duplicates can happen if for example confirmation ACK from the leader was missed.*
> -1 (all) → producer will consider the write successful when all of the in-sync replicas receive the record. Costly in terms of performance. **(Exactly once)** !!! `min.insync.replicas < replics_num`. Duplicates are still possible! => `enable.idempotence`

## Broker

`Kafka clusters` consist of multiple servers for high-availability needs, known as `brokers`. These are the actual machines and instances ***running the Kafka process***. 
Each broker holds a number of partitions and each of these partitions can be either a leader (primary) or a replica for a topic. All writes and reads to a topic go through the leader and the leader coordinates updating replicas with new data. If a leader fails, a replica takes over as the new leader.
![Pasted image 20231019231915](../../../../../_Attachments/Pasted%20image%2020231019231915.png)
!! For fault tolerance partitions can be replicated. The number of replicas / **replication factor** can be configured globally or individually for each topic. 

!! Replicas can be leaders or followers.Producer / Consumers usually work with leader (connects to the broker with stores leader), the followers just sync with leader.
The data that was added to the leader by Producer auto replicates inside cluster (usually async). Since 2.4 Consumers can read from followers => less latency.

!! Kafka auto assign leader ([Consensus Algorythm](../../1.%20Concepts/Consensus%20Algorithms/_Base.md))

> We can increase the number of brokers => scale our system. We can also move partitions between brokers

### Topic

Topics are logical separations for storing messages in a Kafka cluster. 

Kafka retains message records as logs. **Offsets** determine the relative positioning of logs for producers’/consumers consumption. These are stored in the Kafka internal topic **__consumer_offsets.** A topic can have zero or more consumers, as well as it can have zero or more producers.

![](../../../../../_Attachments/Pasted%20image%2020240121154758.png)
#### Partition

Topics are subdivided into **partitions**, the smallest storage unit in the Kafka hierarchy. 

! ! The increase of partitions => increase paralysation during write / read 

Partition is an *==**ordered and immutable sequence of records==*.** Inside each partition, each published event receives an identifier, the **offset**. The offset is used by the consumer to control which events have been already consumed and which event is the next one to consume. But **the consuming order is only guaranteed in the same partition.**

![Pasted image 20231019232015](../../../../../_Attachments/Pasted%20image%2020231019232015.png)

Each partition stores **log files replicated** across nodes distributed into multiple brokers for fault tolerance. 
##### Segments

These log files are called **segments**. A **segment** is simply a ***collection of messages of a partition***. Segment N contains the most recent records and Segment 1 contains the oldest retained records. The segment contains 1 GB of data (`log.segment.bytes`) or 1 week's worth of data (`log.roll.ms` or `log.roll.hours`) whichever is smaller. If this limit is reached, then the segment file is closed and a new segment file is created.

![Pasted image 20231019232112](../../../../../_Attachments/Pasted%20image%2020231019232112.png)
> A partition has only one consumer per consumer group.  
> At any time a single broker will be the leader for a partition. That broker will be responsible for receiving and serving data of that partition.

!! It is impossible to remove anything from log file, yet you can use **Retention Policy**.
## Consumers

> A consumer subscribes to one or more topics. 
> A consumer can be part of only one consumer group.
> Consumer in consumer group can work only with one partition

*If you have more consumers than partitions then some consumers will be idle because they have no partitions to read from. If you have more partitions than consumers then consumers will receive messages from multiple partitions. If you have equal numbers of consumers and partitions, each consumer reads messages in order from exactly one partition.*

Partitions in the Consumers Groups are assigned automatically by Group Coordinator. 

A consumer group is a group of multiple consumers where each consumer present in a group reads data directly from the exclusive partitions. ***In case, the number of consumers is more than the number of partitions, some of the consumers will be in an inactive state.***

Kafka stores on its side current offset for each topic partition => reads always from the last saved position. Each consumer in group after processing messages sends request to save current offset. 

### Rebalancing

Moving partition ownership from one consumer to another is called a `rebalance`.

Default behaviour: all consumers stop reads and wait. (**_Stop-The-World_**)
Other behaviour:
1. Static membership
2. Cooperative Incremental Partition Assignor

! It is recommended to use one consumer group per topic.

# Cross Cluster Replication (XDCR)

Kafka uses MirrorMaker2 for replication. MM2 is a combination of an Apache Kafka source connector and a sink connector.

1. Mirror Source Connector  
2. Mirror CheckPoint Connector  
3. Mirror Heartbeat Connector

MM2 automatically detects new topics and partitions, while also ensuring the topic configurations are synced between clusters.

For our cluster, Kafka MirrorMaker offers geo-replication. Basically, messages are replicated across multiple data centers or cloud regions, with MirrorMaker. So, it can be used in active/passive scenarios for backup and recovery; or also to place data closer to our users, or support data locality requirements.

Other options are **uReplicator** (from Uber), Mirus (from Salesforce), Brooklin (from LinkedIn), and confluent Replicator (from Confluent, not opens sourced).

_Note: Kafka is not a messaging service although it can provide a constrained queueing solution, it will be wise to go for NATS or RabbitMQ as a specialized solution_

# Consistency and Availability

Guarantees hold _as long as you are producing to one partition and consuming from one partition_. 

! All guarantees are off if you are reading from the same partition using two consumers or writing to the same partition using two producers.

Kafka makes the following guarantees about data consistency and availability: 
1. messages sent to a topic partition will be appended to the commit log in the order they are sent
2. a single consumer instance will see messages in the order they appear in the log, 
3. a message is ‘committed’ when all in sync replicas have applied it to their log 
4. any committed message will not be lost, as long as at least one in sync replica is alive

# Metrics to monitor

1. Consumer/Producer Log  
2. Consumer/Producer Throughput  
3. Consumer/producer Error Rate  
4. End to End Latency  
5. Broker CPU / Memory

# References:

1. [What makes Kafka so performant?](What%20makes%20Kafka%20so%20performant?.md)
2. [Mastering Kafka Streams and ksqlDB: Building real-time data systems by example](http://libgen.rs/book/index.php?md5=9F77B9C094AEAACBF48960DB53FCC5D2) (book)
3. [Kafka: The Definitive Guide: Real-Time Data and Stream Processing at Scale](https://www.amazon.pl/Kafka-Definitive-Real-Time-Stream-Processing/dp/1492043087/259-1075280-4270945?pd_rd_w=tVgHa&content-id=amzn1.sym.bc6ff8d8-0c51-4024-8c1a-7a317047da47&pf_rd_p=bc6ff8d8-0c51-4024-8c1a-7a317047da47&pf_rd_r=XBK3HQEY7T10MRPSDJMH&pd_rd_wg=L2G0A&pd_rd_r=67fc1a10-87a2-4b1e-ba0f-63d60c13ebed&pd_rd_i=1492043087&psc=1) (book)
4. ~~[Kafka in a Nutshell](https://sookocheff.com/post/kafka/kafka-in-a-nutshell/)~~
5. ~~[Простым языком об Apache Kafka, как, зачем и почему](https://teletype.in/@abstractart/kafka-for-novices)~~
6. ~~[Kafka за 20 минут. Ментальная модель и как с ней работать](https://habr.com/ru/companies/sbermarket/articles/738634/)~~
7. ~~[What is Kafka and How does it work?](https://www.youtube.com/watch?v=LN_HcJVbySw&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=6) (video)~~
8. [Kafka Exception Handling and Retry Mechanism](https://medium.com/@cobch7/kafka-exception-handling-and-retry-mechanism-a911541321fe)
9. [A Deep Dive into Apache Kafka: Unraveling the Magic Behind the Scenes](https://medium.com/cloud-native-daily/a-deep-dive-into-apache-kafka-unraveling-the-magic-behind-the-scenes-4233e1f00f6c)
10. ~~[Apache Kafka Crash Course](https://www.youtube.com/watch?v=R873BlNVUB4&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=2) (video)~~
11. [Top 8 Kafka Use Cases — Distributed Systems](https://levelup.gitconnected.com/top-8-kafka-use-cases-distributed-systems-d47fc733c7c1)
12. [Important Kafka Interview Questions](https://naveenpn.medium.com/important-kafka-interview-questions-e836e3de41bf)
13. ~~[Kafka Consumer Group is a Brilliant Design Choice and We should Discuss it](https://www.youtube.com/watch?v=e5uAhoT1hhU&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=12) (video)
14. ~~[When to use a Publish-Subscribe System Like Kafka?](https://www.youtube.com/watch?v=posIZrz-m7s&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=7) (video)~~
15. [Hello Kafka World! The complete guide to Kafka with Docker and Python](https://medium.com/big-data-engineering/hello-kafka-world-the-complete-guide-to-kafka-with-docker-and-python-f788e2588cfc)
16. [Паттерны проектирования приложений на Apache Kafka](https://highload.ru/moscow/2019/abstracts/6072) (video)
17. [The Event-Carried State Transfer pattern](https://itnext.io/the-event-carried-state-transfer-pattern-aae49715bb7f)
18. [Apache Kafka как основа для велосипедостроения](https://www.youtube.com/watch?v=cAzblxYZstU&list=PLH-XmS0lSi_wMtn1TsBc2_vv7tBDAf7Qg&index=7) (video)
19. [Kafka, Python и золотая рыбка / Хабр](https://habr.com/ru/post/587592/)
20. [Python микросервисы с Kafka без боли](https://habr.com/ru/post/578916/)
21. [Кафка. "Описание одной борьбы"](https://www.youtube.com/watch?v=m5CDfrQLzrs&list=PLH-XmS0lSi_zTZrols83QSxI3Q96dSbBm&index=10) (video)
22. [How We Process One Billion Events Per Day With Kafka](https://www.metarouter.io/blog-posts/how-we-process-one-billion-events-per-day-with-kafka)
23. [API-First Approach to Kafka Topic Creation](https://doordash.engineering/2023/12/05/api-first-approach-to-kafka-topic-creation/?ref=architecturenotes.co)
24. [KSQLDB for Kafka](https://docs.ksqldb.io/en/latest/operate-and-deploy/how-it-works/)
25. [Kafka и Chronicle Queue](https://habr.com/ru/companies/ruvds/articles/677454/)
26. [Kafka at LinkedIn](https://engineering.linkedin.com/kafka/running-kafka-scale)
27. [Kafka at Pinterest](https://medium.com/pinterest-engineering/how-pinterest-runs-kafka-at-scale-ff9c6f735be)
28. [Kafka at Trello](https://tech.trello.com/why-we-chose-kafka/)
29. [Kafka at Salesforce](https://engineering.salesforce.com/how-apache-kafka-inspired-our-platform-events-architecture-2f351fe4cf63)
30. [Kafka at The New York Times](https://open.nytimes.com/publishing-with-apache-kafka-at-the-new-york-times-7f0e3b7d2077)
31. [Kafka at Yelp](https://engineeringblog.yelp.com/2016/07/billions-of-messages-a-day-yelps-real-time-data-pipeline.html)
32. [Kafka at Criteo](https://medium.com/criteo-labs/upgrading-kafka-on-a-large-infra-3ee99f56e970)
33. [Kafka on Kubernetes at Shopify](https://shopifyengineering.myshopify.com/blogs/engineering/running-apache-kafka-on-kubernetes-at-shopify)
34. [Kafka on PaaSTA: Running Kafka on Kubernetes at Yelp (2 parts)](https://engineeringblog.yelp.com/2022/03/kafka-on-paasta-part-two.html)
35. [Migrating Kafka's Zookeeper with No Downtime at Yelp](https://engineeringblog.yelp.com/2019/01/migrating-kafkas-zookeeper-with-no-downtime.html)
36. [Reprocessing and Dead Letter Queues with Kafka at Uber](https://eng.uber.com/reliable-reprocessing/)
37. [Chaperone: Audit Kafka End-to-End at Uber](https://eng.uber.com/chaperone/)
38. [Finding Kafka throughput limit in infrastructure at Dropbox](https://blogs.dropbox.com/tech/2019/01/finding-kafkas-throughput-limit-in-dropbox-infrastructure/)
39. [Cost Orchestration at Walmart](https://medium.com/walmartlabs/cost-orchestration-at-walmart-f34918af67c4)
40. [InfluxDB and Kafka to Scale to Over 1 Million Metrics a Second at Hulu](https://medium.com/hulu-tech-blog/how-hulu-uses-influxdb-and-kafka-to-scale-to-over-1-million-metrics-a-second-1721476aaff5)
41. ~~[When to use a Publish-Subscribe System Like Kafka?](https://www.youtube.com/watch?v=posIZrz-m7s&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=7) (video)~~
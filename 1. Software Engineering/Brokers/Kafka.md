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
1. Command Query Responsibility Segregation ([[CQRS]])
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

Producers are services/applications that generate messages/data streams. They connect to a Kafka Cluster using a **Broker** or a **Zookeeper** URL. **All events having the same key will be published in the same partition**, which is a way to guarantee the ordering when consuming events. This is done using key hashing by murmur2 algorithm. A message produce by Producer will have following attributes:

1. Key — binary | Optional  
2. Value — binary | Optional  
3. Compression — none | gzip | snappy | zstd | lz4  
4. Headers — Metadata of message | Key: Value | Optional  
5. Partition & Offset  
6. Timestamp

## Consumers

A consumer group is a group of multiple consumers where each consumer present in a group reads data directly from the exclusive partitions. ***In case, the number of consumers is more than the number of partitions, some of the consumers will be in an inactive state.***

Moving partition ownership from one consumer to another is called a `rebalance`.

> A consumer subscribes to one or more topics. 
> A consumer can be part of only one consumer group.

## Topic

Topics are logical separations for storing messages in a Kafka cluster. Kafka retains message records as logs. **Offsets** determine the relative positioning of logs for producers’/consumers consumption. These are stored in the Kafka internal topic **__consumer_offsets.** A topic can have zero or more consumers, as well as it can have zero or more producers.

## Broker

`Kafka clusters` consist of multiple servers for high- availability needs, known as `brokers`. These are the actual machines and instances ***running the Kafka process***. Each broker can host a set of partitions and handle requests to write and read events to these partitions. They also help in message data replicated across partitions.
![[Pasted image 20231019231915.png]]
## Partition

Topics are subdivided into **partitions**, the smallest storage unit in the Kafka hierarchy. Partition is an *==**ordered and immutable sequence of records==*.** Inside each partition, each published event receives an identifier, the **offset**. The offset is used by the consumer to control which events have been already consumed and which event is the next one to consume. But **the consuming order is only guaranteed in the same partition.**
![[Pasted image 20231019232015.png]]

> Partitions are a unit of parallelism for Kafka consumers.

Each partition stores **log files replicated** across nodes distributed into multiple brokers for fault tolerance. 
These log files are called `segments`. A `segment` is simply a ***collection of messages of a partition***. Segment N contains the most recent records and Segment 1 contains the oldest retained records. The segment contains 1 GB of data (`log.segment.bytes`) or 1 week's worth of data (`log.roll.ms` or `log.roll.hours`) whichever is smaller. If this limit is reached, then the segment file is closed and a new segment file is created.
![[Pasted image 20231019232112.png]]
> A partition has only one consumer per consumer group.  
> At any time a single broker will be the leader for a partition. That broker will be responsible for receiving and serving data of that partition.

## Stream Processors

Streams consume an input stream from one or more topics and produce an output stream to one or more output topics, effectively transforming the input streams into output streams. **This is mainly used to transform data mainly in ELT/ETL pipelines.** Kafka provides an integrated Streams API library. This is similar to consumer API.

## Connectors

Connectors are the reusable producers or consumers that connect Kafka topics to existing applications or data systems. 

1. Reduced development effort  
2. Automatic Offset management & Recovering lost messages  
3. Pluggable & Highly available and scalable  
4. Standardization  
5. Retrofitting of source/sink  
6. Reduced maintenance

## Schema Registry

In Kafka, producers and consumers do not communicate with each other directly, instead, they transfer data via Kafka topics. Producers must know how to serialize data and consumers must know how to deserialize it.

A schema Registry is an independent process that runs separately from the Kafka brokers. It stores and retrieves Avro, JSON, Protobuf schemas, etc. Using these schemas it performs verification for data serialization and deserialization.

# Writing strategy

Ack — It denotes the number of brokers that must receive the record before we consider the write as successful. Valid values are 0, 1, and ALL.

> 0 → producer won’t wait for a response from the broker
> 
> 1 → producer will consider write successful when the leader receives the record
> 
> 2 →producer will consider the write successful when all of the in-sync replicas receive the record

 **==Since Kafka producer APIs are idempotent. Write will happen exactly once.==**

# Cross Cluster Replication (XDCR)

Kafka uses MirrorMaker2 for replication. MM2 is a combination of an Apache Kafka source connector and a sink connector.

1. Mirror Source Connector  
2. Mirror CheckPoint Connector  
3. Mirror Heartbeat Connector

MM2 automatically detects new topics and partitions, while also ensuring the topic configurations are synced between clusters.

Other options are **uReplicator** (from Uber), Mirus (from Salesforce), Brooklin (from LinkedIn), and confluent Replicator (from Confluent, not opens sourced).

_Note: Kafka is not a messaging service although it can provide a constrained queueing solution, it will be wise to go for NATS or RabbitMQ as a specialized solution_

# Metrics to monitor

1. Consumer/Producer Log  
2. Consumer/Producer Throughput  
3. Consumer/producer Error Rate  
4. End to End Latency  
5. Broker CPU / Memory

# References:

1. [[What makes Kafka so performant?]]
2. [Mastering Kafka Streams and ksqlDB: Building real-time data systems by example](http://libgen.rs/book/index.php?md5=9F77B9C094AEAACBF48960DB53FCC5D2) (book)
3. [Простым языком об Apache Kafka, как, зачем и почему](https://teletype.in/@abstractart/kafka-for-novices)
4. [Kafka за 20 минут. Ментальная модель и как с ней работать](https://habr.com/ru/companies/sbermarket/articles/738634/)
5. [Чем различаются Kafka и RabbitMQ: простыми словами](https://habr.com/ru/companies/innotech/articles/698838/)
6. ~~[What is Kafka and How does it work?](https://www.youtube.com/watch?v=LN_HcJVbySw&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=6) (video)~~
7. [A Deep Dive into Apache Kafka: Unraveling the Magic Behind the Scenes](https://medium.com/cloud-native-daily/a-deep-dive-into-apache-kafka-unraveling-the-magic-behind-the-scenes-4233e1f00f6c)
8. ~~[Apache Kafka Crash Course](https://www.youtube.com/watch?v=R873BlNVUB4&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=2) (video)~~
9. [Top 8 Kafka Use Cases — Distributed Systems](https://levelup.gitconnected.com/top-8-kafka-use-cases-distributed-systems-d47fc733c7c1)
10. [Important Kafka Interview Questions](https://naveenpn.medium.com/important-kafka-interview-questions-e836e3de41bf)
11. ~~[Kafka Consumer Group is a Brilliant Design Choice and We should Discuss it](https://www.youtube.com/watch?v=e5uAhoT1hhU&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=12) (video)
12. [When to use a Publish-Subscribe System Like Kafka?](https://www.youtube.com/watch?v=posIZrz-m7s&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=7) (video)
13. [Hello Kafka World! The complete guide to Kafka with Docker and Python](https://medium.com/big-data-engineering/hello-kafka-world-the-complete-guide-to-kafka-with-docker-and-python-f788e2588cfc)
14. [Паттерны проектирования приложений на Apache Kafka](https://highload.ru/moscow/2019/abstracts/6072) (video)
15. [The Event-Carried State Transfer pattern](https://itnext.io/the-event-carried-state-transfer-pattern-aae49715bb7f)
16. [Apache Kafka как основа для велосипедостроения](https://www.youtube.com/watch?v=cAzblxYZstU&list=PLH-XmS0lSi_wMtn1TsBc2_vv7tBDAf7Qg&index=7) (video)
17. [Kafka, Python и золотая рыбка / Хабр](https://habr.com/ru/post/587592/)
18. [Python микросервисы с Kafka без боли](https://habr.com/ru/post/578916/)
19. [Кафка. "Описание одной борьбы"](https://www.youtube.com/watch?v=m5CDfrQLzrs&list=PLH-XmS0lSi_zTZrols83QSxI3Q96dSbBm&index=10) (video)
20. [How We Process One Billion Events Per Day With Kafka](https://www.metarouter.io/blog-posts/how-we-process-one-billion-events-per-day-with-kafka)
21. [KSQLDB for Kafka](https://docs.ksqldb.io/en/latest/operate-and-deploy/how-it-works/)
22. [Kafka и Chronicle Queue](https://habr.com/ru/companies/ruvds/articles/677454/)
23. [Kafka at LinkedIn](https://engineering.linkedin.com/kafka/running-kafka-scale)
24. [Kafka at Pinterest](https://medium.com/pinterest-engineering/how-pinterest-runs-kafka-at-scale-ff9c6f735be)
25. [Kafka at Trello](https://tech.trello.com/why-we-chose-kafka/)
26. [Kafka at Salesforce](https://engineering.salesforce.com/how-apache-kafka-inspired-our-platform-events-architecture-2f351fe4cf63)
27. [Kafka at The New York Times](https://open.nytimes.com/publishing-with-apache-kafka-at-the-new-york-times-7f0e3b7d2077)
28. [Kafka at Yelp](https://engineeringblog.yelp.com/2016/07/billions-of-messages-a-day-yelps-real-time-data-pipeline.html)
29. [Kafka at Criteo](https://medium.com/criteo-labs/upgrading-kafka-on-a-large-infra-3ee99f56e970)
30. [Kafka on Kubernetes at Shopify](https://shopifyengineering.myshopify.com/blogs/engineering/running-apache-kafka-on-kubernetes-at-shopify)
31. [Kafka on PaaSTA: Running Kafka on Kubernetes at Yelp (2 parts)](https://engineeringblog.yelp.com/2022/03/kafka-on-paasta-part-two.html)
32. [Migrating Kafka's Zookeeper with No Downtime at Yelp](https://engineeringblog.yelp.com/2019/01/migrating-kafkas-zookeeper-with-no-downtime.html)
33. [Reprocessing and Dead Letter Queues with Kafka at Uber](https://eng.uber.com/reliable-reprocessing/)
34. [Chaperone: Audit Kafka End-to-End at Uber](https://eng.uber.com/chaperone/)
35. [Finding Kafka throughput limit in infrastructure at Dropbox](https://blogs.dropbox.com/tech/2019/01/finding-kafkas-throughput-limit-in-dropbox-infrastructure/)
36. [Cost Orchestration at Walmart](https://medium.com/walmartlabs/cost-orchestration-at-walmart-f34918af67c4)
37. [InfluxDB and Kafka to Scale to Over 1 Million Metrics a Second at Hulu](https://medium.com/hulu-tech-blog/how-hulu-uses-influxdb-and-kafka-to-scale-to-over-1-million-metrics-a-second-1721476aaff5)
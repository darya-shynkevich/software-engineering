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

# [[What makes Kafka so performant?]]

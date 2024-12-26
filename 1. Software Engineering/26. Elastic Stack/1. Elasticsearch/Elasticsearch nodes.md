## 1. Master-eligible nodes

**Master-eligible nodes** take part in the master election process. At any point in time, a single node is elected to be the active master. 

The active master node keeps track of 
1. other nodes in the cluster, 
2. creation or deletion of indices, 
3. shards being allocated to nodes based on requirements/constraints, 
4. cluster settings being applied, 
5. and more.

The master role is generally not very resource-intensive and **can be co-located** on a node running other roles in smaller clusters. Running the master role on a dedicated host makes sense when the following are true:
1. Existing nodes have high resource utilization, especially when servicing heavy indexing/search operations.
2. The cluster contains 10 or more nodes (as a general heuristic), where the administrative overhead on the masters requires dedicated resources.

**NOTE:** For **high availability**, it is important to have **more than one master-eligible node** in case of hardware failures => iit is recommended to have three master-eligible nodes in a highly available cluster.

## 2. Voting-only nodes

Master-eligible nodes, when they're not elected, do not do any useful work. In the interest of **reducing infrastructure costs**, one of the master-eligible nodes can be replaced with a voting-only node. A **voting-only node** will vote in elections (acting as a tie-breaker) but **will not be elected as the master node**. The voting-only role is lightweight and can be serviced by a node with minimal CPU and memory. It **can also be run on a data or ingest node if required.**

## 3. Data nodes

**Data nodes** host shards that make up your indices and respond to read/write requests.

Depending on the amount of data being indexed, data nodes need sufficient **JVM heap**, **CPU**, and **disk storage**. 

More data nodes can be added to the cluster to **horizontally scale** the indexing/search throughput and perform data retention.

Data nodes can be part of a specific tier in the cluster to take advantage of different hardware profiles and price factors.
![](Pasted%20image%2020241225232632.png)
Data can be moved across the different data tiers throughout the life cycle of an index

## 4. Ingest nodes

**Ingest nodes** run any **ingest pipelines** associated with an indexing request. Ingest pipelines contain processors that can **transform incoming documents** before they are indexed and stored on data nodes. 

Ingest nodes **can be run on the same host as the data nodes** if
1. an ETL tool such as [[Logstash]] is used
2. the transformations using ingest pipelines are minimal.

**Dedicated ingest nodes** can be used if **resource-intensive ingest pipelines** are required.

## 5. Coordinator nodes

**All Elasticsearch nodes perform the coordination role by default.** 

Coordinator nodes
1. route search/indexing requests to the appropriate data node
2. combine search results from multiple shards before returning them to the client.

Given that the coordinator role is fairly lightweight and all the nodes in the cluster must perform this role in some capacity, having **dedicated coordinator nodes is generally not recommended**. 

**Performance bottlenecks** on ingestion and search can usually be alleviated by adding more
1. data nodes
2. ingest nodes (if utilising heavy ingest pipelines)
rather than using dedicated coordinator nodes.

## 6. Machine learning nodes

Machine learning nodes run machine learning jobs and handle machine learning-related API requests. Machine learning jobs can be fairly resource-intensive (mostly CPU-bound since models have memory limits) and **can benefit from running on a dedicated node.**

Machine learning jobs **may generate resource-intensive search requests** on data nodes when feeding input to models. Additional machine learning nodes can be added to scale the capacity for running jobs as required, while **data nodes need to be scaled to alleviate load from ML input operations**.

NOTE: Machine learning is a paid subscription feature. A trial license can be enabled if you wish to learn and test machine learning functionality.


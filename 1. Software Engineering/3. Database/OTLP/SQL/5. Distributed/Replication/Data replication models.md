# 1. Single leader / primary-secondary replication

(+) excellent for read heavy applications
(+) read resilient

(-) bad choice for write heavy applications
(-) comes with inconsistency if we use asynchronous replication
(-) if primary fails => updates before sync will be lost

In **primary-secondary replication,** data is replicated across multiple nodes. One node is designated as the primary. It’s responsible for processing any writes to data stored on the cluster. It also sends all the writes to the secondary nodes and keeps them in sync.

Primary-secondary replication ***is appropriate when our workload is read-heavy***. To better scale with increasing readers, we can add more followers and distribute the read load across the available followers. However, replicating data to many followers can make a primary bottleneck. Additionally, primary-secondary replication ***is inappropriate if our workload is write-heavy.***

Another advantage of primary-secondary replication is that it’s ***read resilient***. Secondary nodes can still handle read requests in case of primary node failure. Therefore, it’s a helpful approach for read-intensive applications.

Replication via this approach ***comes with inconsistency if we use asynchronous replication***. Clients reading from different replicas may see inconsistent data in the case of failure of the primary node that couldn’t propagate updated data to the secondary nodes. So, if the primary node fails, any missed updates not passed on to the secondary nodes can be lost.
## What happens when the primary node fails?

In case of failure of the primary node, a secondary node can be appointed as a primary node, which speeds up the process of recovering the initial primary node. There are two approaches to select the new primary node: manual and automatic.

In a **manual approach**, an operator decides which node should be the primary node and notifies all secondary nodes.

In an **automatic approach**, when secondary nodes find out that the primary node has failed, they appoint the new primary node by conducting an election known as a [[Leader election]].
# 2. Multi-leader replication

(+) can be useful in applications in which we can continue work offline and later sync our data
(+) better performance and scalability than single leader replication

(-) conflicts when the same data changes

As discussed above, single leader replication using asynchronous replication has a drawback. There’s only one primary node, and all the writes have to go through it, which limits the performance. In case of failure of the primary node, the secondary nodes may not have the updated database.

**Multi-leader replication** is an alternative to single leader replication. ***There are multiple primary nodes that process the writes and send them to all other primary and secondary nodes to replicate.*** 
! This type of replication is used in databases along with external tools like the [[Tungsten Replicator]] for MySQL.

This kind of replication is quite useful in applications in which we can continue work even if we’re offline — for example, a calendar application in which we can set our meetings even if we don’t have access to the internet. Once we’re online, it replicates its changes from our local database (our mobile phone or laptop acts as a primary node) to other nodes.

![](../../../../../../_Attachments/Pasted%20image%2020240119194307.png)
## Conflict

Multi-leader replication gives better performance and scalability than single leader replication, but it also has a significant disadvantage. Since all the primary nodes concurrently deal with the write requests, they ***may modify the same data, which can create a conflict between them***. 
For example, suppose the same data is edited by two clients simultaneously. In that case, their writes will be successful in their associated primary nodes, but when they reach the other primary nodes asynchronously, it creates a conflict.

Conflicts can result in different data at different nodes. These should be handled efficiently without losing any data.

![](../../../../../../_Attachments/Pasted%20image%2020240119194515.png)
### 1. Conflict avoidance

A simple strategy to deal with conflicts is to prevent them from happening in the first place. Conflicts can be avoided ***if the application can verify that all writes for a given record go via the same leader***.

However, the conflict may still occur if a user moves to a different location and is now near a different data center. If that happens, we need to reroute the traffic. In such scenarios, the conflict avoidance approach fails and results in concurrent writes.
### 2. Last-write-wins

Using their local clock, all nodes assign a timestamp to each update. When a conflict occurs, the update with the latest timestamp is selected.

This approach can also create difficulty because the clock synchronisation across nodes is challenging in distributed systems. There’s clock skew that can result in data loss.
### 3. Custom logic

In this approach, we can write our own logic to handle conflicts according to the needs of our application. This custom logic can be executed on both reads and writes. When the system detects a conflict, it calls our custom conflict handler.

## Multi-leader replication topologies

There are many topologies through which multi-leader replication is implemented, such as circular topology, star topology, and all-to-all topology. The most common is the all-to-all topology. In star and circular topology, there’s again a similar drawback that if one of the nodes fails, it can affect the whole system. That’s why all-to-all is the most used topology.

# 3. Peer-to-peer / leaderless replication

(-) inconsistency

In primary-secondary replication, the primary node is a bottleneck and a single point of failure. Moreover, it helps to achieve read scalability but fails to provide write scalability. The **peer-to-peer replication** model resolves these problems by not having a single primary node. All the nodes have equal weightage and can accept read and write requests. This replication scheme can be found in the [[Cassandra]] database.

![](../../../../../../_Attachments/Pasted%20image%2020240119195006.png)

Like primary-secondary replication, this replication can also yield inconsistency. This is because when several nodes accept write requests, it may lead to concurrent writes. A helpful approach used for solving write-write inconsistency is called **[[Quorum]]**.



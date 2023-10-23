***Redis** (“**RE**mote **DI**ctionary **S**ervice”) is an open-source key-value database server.*

[[How Redis works internally]]

![[Pasted image 20230605224617.png]]
*Rather than iterating over, sorting, and ordering rows, what if the data was in data structures you wanted from the ground up? Early on, it was used much like Memcached, but as Redis improved, it became viable for many other use cases, including publish-subscribe mechanisms, streaming, and queues.*

![[Pasted image 20230605224913.png]]

Primarily, Redis is an in-memory database *==used as a cache in front of another "real" database like MySQL or PostgreSQL to help improve application performance.==* It leverages the speed of memory and alleviates load off the central application database for:
- Data that changes infrequently and is requested often
- Data that is less mission-critical and is frequently evolving.

![[Pasted image 20230605225013.png]]

However, for many use cases, *==Redis offers enough guarantees that it can be used as a full-fledged primary database==*. Coupled with Redis plug-ins and its various High Availability (HA) setups, Redis as a database  has become incredibly useful for certain scenarios and workloads.

Another important aspect is that *==Redis blurred the lines between a cache and datastore==*. Important note to understand here is that ***reading and manipulating data in memory is much faster than anything possible in traditional datastores using SSDs or HDDs***.
![[Pasted image 20230605225125.png]]
Important latency and bandwidth numbers every software engineer to should be aware of.

Originally Redis was most commonly compared to [[Memcached]], which lacked any nonvolatile persistence at the time.

***Here is a current breakdown of capabilities between these two caches.***

![[Pasted image 20230605225352.png]]


## Redis Architectures

Depending on your use case and scale, you can decide to use one setup or another.

### 1. Single Redis Instance

![[Pasted image 20230605225601.png]]
Single Redis instance is the most straightforward deployment of Redis. It allows users to set up and run small instances that can help them grow and speed up their services. However, this deployment isn't without shortcomings. *==For example, if this instance fails or is unavailable, all client calls to Redis will fail and therefore degrade the system's overall performance and speed==*.

Given enough memory and server resources, this instance can be powerful. *==A scenario primarily used for caching could result in a significant performance boost with minimal setup==*. Given enough system resources, you could deploy this Redis service on the same box the application is running.

Understanding a few Redis concepts on managing data within the system is essential. *Commands sent to Redis are first processed in memory. Then, if persistence is set up on these instances, there is a forked process on some interval that facilitates data persistence RDB (very compact point-in-time representation of Redis data) snapshots or AOF (append-only files).*

These two flows allow Redis to have long-term storage, support various replication strategies, and enable more complicated topologies. *If Redis isn't set up to persist data, data is lost in case of a restart or failover.* If the persistence is enabled on a restart, it loads all of the data in the RDB snapshot or AOF back into memory, and then the instance can support new client requests.

### 2. Redis HA

![[Pasted image 20230605225928.png]]

Another popular setup with Redis is the main deployment with a secondary deployment that is kept in sync with replication.  *==As data is written to the main instance it sends copies of those commands, to a replica client output buffer for secondary instances which facilitates replication.==* The secondary instances can be one or more instances in your deployment. *==These instances can help scale reads from Redis or provide failover in case the main is lost.==*

*In these HA (**High availability**) systems, **it is essential to not have a single point of failure so systems can recover gracefully and quickly**. This results in reliable crossover, so data isn't lost during the transition from primary to secondary, in addition to automatically detecting failure and recovery from it.*

### 3. Redis Replication

Every main instance of Redis has a replication ID and an offset. These two pieces of data are critical to figure out a point in time ==*where a replica can continue its replication process or to determine if it needs to do a complete sync*.== This offset is incremented for every action that happens on the main Redis deployment.

*More explicitly, when the Redis replica instance is just a few offsets behind the main instance, it receives the remaining commands from the primary, which is then replayed on its dataset until it is in sync. If the two instances cannot agree on a replication ID or the offset is unknown to the main instance, the replica will then request a full synchronization. This involves a primary instance creating a new RDB snapshot and sending it over to the replica. While this transfer is happening, the main instance is buffering all the intermediate updates between the snapshot cut-off and current offset to send to the secondary once it is in sync with the snapshot. Once complete, replication can continue as normal.*

If an instance has the same replication ID and offset, they have precisely the same data. Now you may be wondering why a replication ID is required. When a Redis instance is promoted to primary or restarts from scratch as a primary, it is given a new replication ID. This is used to infer the prior primary instance from which this newly promoted secondary was replicating. This allows for the ability to perform a partial synchronization (with other secondaries) since the new primary instance remembers its old replication ID.

*For example, two instances, primary and secondary, have the identical replication ID but offsets that differ by a few hundred commands, meaning that if those were replayed on the instance that is just behind in offset, they would have the same dataset. Now if the replication IDs differ entirely, and when we are unaware of the previous replication ID (no common ancestor) of the newly demoted (and rejoining) secondary. We will need to perform an expensive full sync.*
*Alternatively if we are aware of previous replication ID we can then reason about how to get the data in sync since we are able to reason about common ancestor they both shared and the offset is again meaningful for a partial sync.*

### 4. Redis Sentinel

![[Pasted image 20230605230744.png]]

Sentinel is a distributed system. As with all distributed systems, Sentinel comes with several advantages and disadvantages. *==Sentinel is designed in a way where there is a cluster of sentinel processes working together to coordinate state to provide high availability for Redis.==* After all you wouldn't want the system protecting you from failure to have its own single point of failure.

Sentinel is responsible for a few things. First, it ensures that the current main and secondary instances are functional and responding. This is necessary because *==sentinel (with other sentinel processes) can alert and act on situations where the main and/or secondary nodes are lost.==* Second, it *==serves a role in service discovery==* much like Zookeeper and Consul in other systems. *So when a new client attempts to write something to Redis, Sentinel will tell the client what current main instance is.*

*==So sentinels are constantly monitoring availability and sending out that information to clients so they are able to react to them if they indeed do failover.==*

Here are its responsibilities:
1. Monitoring **—** ensuring main and secondary instances are working as expected.
2. Notification **—** notify system admins about occurrences in the Redis instances.
3. Failover management — Sentinel nodes can start a failover process if the primary instance isn't available and enough (quorum of) nodes agree that is true.
4. Configuration management — Sentinel nodes also serve as a point of discovery of the current main Redis instance.

*==Using Redis Sentinel in this way allows for failure detection.==* This detection involves multiple sentinel processes agreeing that current main instance is no longer available. This agreement process is called [[Quorum]]. This allows for increased robustness and protection against one machine misbehaving and being unable to reach the main Redis node.

You can deploy Redis Sentinel in several ways. Honestly to make any sane recommendation I would need more context than I currently have about your system. As general guidance *I would recommend running a sentinel node along aside each of your application servers (if possible) so you also don't need to factor in network reachability differences between sentinel nodes and clients who are actually using Redis.* You can run Sentinel alongside the Redis instances or even on independent nodes, but that complicates things in different ways. I recommend at least running three nodes with a quorum of at least two. Here is a simple chart breaking down numbers of servers in a cluster and associated quorum and tolerated failures that are sustainable.

*Table of number of servers and quorum with number of tolerated failures.*

![[Pasted image 20230605231838.png]]

1. What if the sentinel nodes fall out of quorum?
2. What if there is a network split which puts the old main instance in the minority group? What happens to those writes? (Spoiler: they are lost when the system recovers fully)
3. What happens if the network topologies of sentinel nodes and client nodes (application nodes) are misaligned?

*There are a few ways to mitigate the level of losses if you force the main instance to replicate writes to a minimum of one secondary instance. Remember, all Redis replication is asynchronous and has its trade-offs. So it will need to independently track acknowledgement and if they aren't confirmed by at least one secondary, the main instance will stop accepting writes.*

### 5. Redis Cluster
![[Pasted image 20230605232305.png]]

*==Each Redis instance in the cluster is considered a shard of the data as a whole.==*

Redis Cluster commonly uses *==algorithmic sharding==* to determine which Redis instance is holding specific data.

In Redis Cluster, a *==deterministic hash function==* is used to determine the shard for a given key. The key is hashed and the result is modded by the number of shards to locate its location. This function ensures that the key will always exist in the same shard.

###### **Resharding**

When new shards are introduced, the keys may be mapped to different shards. It's not practical to move the data around to reflect the new mapping, as it would negatively affect the availability of the Redis Cluster.

Redis Cluster uses *=="Hashslot"==* to map all data and spread it across the cluster. With 16k hashslots, when new shards are added, hashslots can be moved between the systems, simplifying the process of adding new primary instances to the cluster.

***Example:***
*M1 contains hashslots from 0 to 8191.
M2 contains hashslots from 8192 to 16383.*

*So to map `foo`, we take a deterministic hash of the key (foo) and mod it by the number of hash slots(16K), leading to a mapping of M2. Now let's say we add a new instance, M3. The new mappings would be*

*M1 contains hashslots from 0 to 5460.
M2 contains hashslots from 5461 to 10922.
M3 contains hashslots from 10923 to 16383.*

*All the keys that mapped the hashslots in M1 that are now mapped to M2 would need to move. But the hashing for the individual keys to hashslots wouldn't need to move because they have already been divided up across hashslots. So this one level of misdirection solves the resharding issue with algorithmic sharding.*

### Gossiping

*In Redis Cluster, all the nodes in the cluster constantly communicate with each other to know which shards are available and can serve requests. If a sufficient number of shards agree that a primary instance is not responsive, they can decide to promote one of its secondary instances to primary to keep the cluster running smoothly. It is important to configure the number of nodes needed to trigger this properly to avoid a situation called split-brain, where the cluster is split if it cannot break the tie when both sides of a partition are equal. A good practice is to have an odd number of primary nodes and two replicas each for the most robust setup.*

## Redis Persistence Models

![[Pasted image 20230606192017.png]]

### No persistence

This is the fastest way to run Redis and has no durability guarantees.

### RDB Files

**RDB** (Redis Database): The RDB persistence *performs point-in-time snapshots of your dataset at specified intervals*.

The main downside to this mechanism is that *data between snapshots will be lost*. In addition, this storage mechanism also relies on forking the main process, and in a larger dataset, this may lead to a momentary delay in serving requests. *That being said, RDB files are much faster being loaded in memory than AOF.*

### AOF

**AOF** (Append Only File): *The AOF persistence logs every write operation the server receives that will be played again at server startup, reconstructing the original dataset.*

This way of ensuring persistence is much more durable than RDB snapshots since it is an append-only file. As operations happen, we buffer them to the log, but they aren't persisted yet. *This log consists of the actual commands we ran in order for replay when needed.*

Then when possible, we flush it to disk with [[fsync]] (when this runs is configurable), it will be persisted. *The downside is that the format isn't compact and uses more disk than RDB files.*

### Why not both?

**RDB + AOF**: It is possible to combine AOF and RDB in the same Redis instance. If durability in exchange for some speed is a tradeoff, you are willing to make it. I think this is an acceptable way to set up Redis. *In the case of a restart, remember that if both are enabled, Redis will use AOF to reconstruct the data since it's the most complete.*

## Forking

*How redis uses forking for point in time snapshots*

![[Pasted image 20230606192704.png]]

*[Forking](https://en.wikipedia.org/wiki/Fork_(system_call)?ref=architecturenotes.co) is a way for operating systems to create new processes by creating copies of themselves.* With this, you get a new process ID and a few other bits of information and handles, so the newly forked process (child) can talk to the original process parent.

When you fork a process, the parent and child share memory, and in that child process Redis begins the snapshotting (Redis) process. This is made possible by a memory sharing technique called *[[Copy-on-write]]* — *which passes references to the memory at the time the fork was created. If no changes occur while the child process is persisting to disk, no new allocations are made.*

In the case where there are changes, the kernel keeps track of references to each page, and if there are more than one to specific page the changes are written to new pages. The child process is fully unaware of the change and has consistent memory snapshot. Therefore only fraction of the memory is used and we are able to achieve a point in time snapshot of potentially gigabytes of memory extremely quickly and efficiently!

*It's like taking a picture of something that hasn't moved - you don't need to take a whole new picture, just the parts that have changed.*

## Points to discuss:

1. What us `fsync` ?
2. How `copy-on-write` works?
3. How exactly `forking` works?
4. Do clients wait for persistent? 

## References:

1. https://architecturenotes.co/redis/ 
> Clients send each write to several nodes, and read from several nodes in parallel in order to detect and correct nodes with stale data.

(-) inconsistency

In primary-secondary replication, the primary node is a bottleneck and a single point of failure. Moreover, it helps to achieve read scalability but fails to provide write scalability. The **peer-to-peer replication** model resolves these problems by not having a single primary node. All the nodes have equal weightage and can accept read and write requests. 

Like primary-secondary replication, this replication can also yield inconsistency. This is because when several nodes accept write requests, it may lead to concurrent writes. A helpful approach used for solving write-write inconsistency is called **[[Quorum]]**.

! If you have `n` replicas, and you choose w and `r` such that `w + r > n`, you can generally expect every read to return the most recent value written for a key. This is the case because the set of nodes to which you’ve written and the set of nodes from which you’ve read must overlap.

Examples:
1. DynamoDB
2. [Cassandra](../../../../NoSQL/Types/Columnar%20Databases/Cassandra.md)
3. [[Riak]]
4. [[Voldemort]]
![](../../../../../../../_Attachments/Pasted%20image%2020240119195006.png)

! read requests are also sent to several nodes in parallel. The cli‐ ent may get different responses from different nodes; i.e., the up-to-date value from one node and a stale value from another.
## Read repair and anti-entropy

**Read repair:** When a client makes a read from several nodes in parallel, it can detect any stale responses. The client sees that replica 3 has a stale value and writes the newer value back to that replica. This approach works well for values that are frequently read.

**Anti-entropy process:** In addition, some datastores have a background process that constantly looks for differences in the data between replicas and copies any missing data from one replica to another. Unlike the replication log in leader-based replication, this anti-entropy process does not copy writes in any particular order, and there may be a significant delay before data is copied.
	[[Voldemort]] does not have this feature => values that are rarely read may be missing from some replicas and thus have reduced durability, because read repair is only performed when a value is read by the application.

# [Quorums](../../../../../../2.%20Architecture/3.%20API/Concepts/Quorum.md) for reading and writing

If there are `n` replicas, every write must be confirmed by `w` nodes to be considered successful, and we must query at least `r` nodes for each read. (In our example, `n = 3`, `w = 2`, `r = 2`.) As long as `w + r > n`, we expect to get an up-to-date value when reading, because at least one of the `r` nodes we’re reading from must be up to date. Reads and writes that obey these `r` and `w` values are called [Quorum](../../../../../../2.%20Architecture/3.%20API/Concepts/Quorum.md) reads and writes. You can think of `r` and `w` as the minimum number of votes required for the read or write to be valid.

> In Dynamo-style databases, the parameters `n`, `w`, and `r` are typically configurable. A common choice is to make `n` an odd number (typically `3` or `5`) and to set `w = r = (n + 1) / 2` (rounded up). However, you can vary the numbers as you see fit. For example, a workload with few writes and many reads may benefit from setting `w = n` and `r = 1`. This makes reads faster, but has the disadvantage that just one failed node causes all database writes to fail.

Normally, reads and writes are always sent to all `n` replicas **in parallel**. The parameters `w` and `r` determine how many nodes we wait for — i.e., how many of the `n` nodes need to report success before we consider the read or write to be successful.

If fewer than the required `w` or `r` nodes are available, writes or reads return an error. A node could be unavailable for many reasons: 
1. because the node is down (crashed, powered down), 
2. due to an error executing the operation (can’t write because the disk is full), 
3. due to a network interruption between the client and the node, 
4. or for any number of other reasons. 
5. 
We only care whether the node returned a successful response and **don’t need to distinguish between different kinds of fault**.

# Limitations of [Quorum](../../../../../../2.%20Architecture/3.%20API/Concepts/Quorum.md) Consistency

With a smaller (`w + r <= n`) `w` and `r` you are more likely to read stale values, because it’s more likely that your read didn’t include the node with the latest value. On the upside, this configuration allows lower latency and higher availability: if there is a network interruption and many replicas become unreachable, there’s a higher chance that you can continue processing reads and writes. Only after the number of reachable replicas falls below `w` or `r` does the database become unavailable for writing or reading, respectively.

However, even with `w + r > `n, there are likely to be edge cases where stale values are returned. These depend on the implementation, but possible scenarios include:

1. If a sloppy quorum is used the `w` writes may end up on different nodes than the `r` reads, so there is no longer a guaranteed overlap between the `r` nodes and the `w` nodes
2. If two writes occur concurrently, it is not clear which one happened first. In this case, the only safe solution is to merge the concurrent writes. If a winner is picked based on a timestamp (last write wins), writes can be lost due to clock skew.
3. If a write happens concurrently with a read, the write may be reflected on only some of the replicas. In this case, it’s undetermined whether the read returns the old or the new value.
4. If a write succeeded on some replicas but failed on others (for example because the disks on some nodes are full), and overall succeeded on fewer than `w` replicas, it is not rolled back on the replicas where it succeeded. This means that if a write was reported as failed, subsequent reads may or may not return the value from that write.
5. If a node carrying a new value fails, and its data is restored from a replica carrying an old value, the number of replicas storing the new value may fall below `w`, breaking the quorum condition.
### Sloppy Quorums and Hinted Handoff

Databases with appropriately configured quorums can tolerate the failure of individual nodes without the need for failover. They can also tolerate individual nodes going slow, because requests don’t have to wait for all n nodes to respond—they can return when `w` or `r` nodes have responded. These characteristics make databases with leaderless replication appealing for use cases that require **high availability** and **low latency**, and that can tolerate **occasional stale reads**.

In a large cluster (with significantly more than `n` nodes) it’s likely that the client can connect to some database nodes during the network interruption, just not to the nodes that it needs to assemble a quorum for a particular value:
1. Is it better to **return errors** to all requests for which we cannot reach a quorum of `w` or `r` nodes?
2. Or should we **accept writes anyway**, and write them to some nodes that are reachable but aren’t among the `n` nodes on which the value usually lives?

The latter is known as a **sloppy quorum**: writes and reads still require `w` and `r` successful responses, but those may include nodes that are not among the designated `n` “home” nodes for a value. *By analogy, if you lock yourself out of your house, you may knock on the neighbour’s door and ask whether you may stay on their couch temporarily.*

Once the network interruption is fixed, any writes that one node temporarily accepted on behalf of another node are sent to the appropriate “home” nodes. This is called **hinted handoff**. (Once you find the keys to your house again, your neighbour politely asks you to get off their couch and go home.)

Sloppy quorums are particularly useful for increasing write availability: as long as any `w` nodes are available, the database can accept writes. However, this means that even when `w + r > n`, you cannot be sure to read the latest value for a key, because the latest value may have been temporarily written to some nodes outside of `n`.

# Multi-datacenter operation

Leaderless replication is also suitable for multi-datacenter operation, since it is designed to tolerate conflicting concurrent writes, network interruptions, and latency spikes.

*[[Cassandra]] and [[Voldemort]] implement their multi-datacenter support within the normal leaderless model: the number of replicas n includes nodes in all datacenters, and in the configuration you can specify how many of the n replicas you want to have in each datacenter. Each write from a client is sent to all replicas, regardless of datacenter, but the client usually only waits for acknowledgment from a quorum of nodes within its local datacenter so that it is unaffected by delays and interruptions on the cross-datacenter link. The higher-latency writes to other datacenters are often configured to happen asynchronously, although there is some flexibility in the configuration*

*[[Riak]] keeps all communication between clients and database nodes local to one datacenter, so `n` describes the number of replicas within one datacenter. Cross-datacenter replication between database clusters happens asynchronously in the background, in a style that is similar to multi-leader replication.*

# Detecting Concurrent Writes

### 1. Last write wins (discarding concurrent writes)

Even though the writes don’t have a natural ordering, we can force an arbitrary order on them. For example, we can attach a timestamp to each write, pick the biggest timestamp as the most “recent,” and discard any writes with an earlier timestamp.

> This conflict resolution algorithm, called last write wins (LWW), is the only supported conflict resolution method in [[Cassandra]], and an optional feature in [[Riak]].

! LWW achieves the goal of **eventual convergence**, but **at the cost of durability**: if there are several concurrent writes to the same key, even if they were all reported as successful to the client (because they were written to w replicas), only one of the writes will survive and the others will be silently discarded.

The only safe way of using a database with LWW is to ensure that a key is only written once and thereafter treated as immutable, thus avoiding any concurrent updates to the same key.

> For example, a recommended way of using [[Cassandra]] is to use a UUID as the key, thus giving each write operation a unique key.

### 2. The “happens-before” relationship and concurrency

An operation `A` happens before another operation `B` if `B` knows about `A`, or depends on `A`, or builds upon `A` in some way. Whether one operation happens before another operation is the key to defining what concurrency means. In fact, we can simply say that **two operations are concurrent if neither happens before the other** (i.e., neither knows about the other.

=> Thus, whenever you have two operations `A` and `B`, there are three possibilities: either `A` happened before `B`, or `B` happened before `A`, or `A` and `B` are concurrent.

1. The server maintains a *version number for every key*, increments the version number every time that key is written, and stores the new version number along with the value written.
2. When a client reads a key, the server returns all values that have not been overwritten, as well as the latest version number. A client must read a key before writing.
3. When a client writes a key, it must include the version number from the prior read, and it must merge together all values that it received in the prior read. (The response from a write request can be like a read, returning all current values, which allows us to chain several writes like in the shopping cart example.)
4. When the server receives a write with a particular version number, it can overwrite all values with that version number or below (since it knows that they have been merged into the new value), but it must keep all values with a higher version number (because those values are concurrent with the incoming write).
### 3. Merging concurrently written values

This algorithm ensures that no data is silently dropped, but it unfortunately requires that the clients do some extra work: if several operations happen concurrently, clients have to clean up afterward by merging the concurrently written values. 

> [[Riak]] calls these concurrent values siblings.

! Merging sibling values is essentially the same problem as conflict resolution in multi- leader replication, which we discussed previously. 

> [[Riak]]’s datatype support uses a family of data structures called CRDTs that can automatically merge siblings in sensible ways, including preserving deletions.
### 4. Version vectors

The collection of version numbers from all the replicas is called a **version vector**. 

> A few variants of this idea are in use, but the most interesting is probably the **dotted version vector,** which is used in [[Riak]]. 

Version vectors are sent from the database replicas to clients when values are read, and need to be sent back to the database when a value is subsequently written. The version vector allows the database to distinguish between overwrites and concurrent writes.

Also, like in the single-replica example, the application may need to merge siblings. The version vector structure ensures that it is safe to read from one replica and subsequently write back to another replica.


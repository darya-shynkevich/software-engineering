### Functional requirements

Typically, key-value stores are expected to offer functions such as `get` and `put`. However, what sets this particular key-value store system apart is its distinct characteristics, explained as follows:

1. **Configurable service**: Some applications might have a tendency to trade strong consistency for higher availability. We need to provide a configurable service so that different applications could use a range of consistency models. We need tight control over the trade-offs between availability, consistency, cost-effectiveness, and performance.
	   **Note:** Such configurations can only be performed when instantiating a new key-value store instance and cannot be changed dynamically when the system is operational.
2. **Ability to always write (when we picked “A” over “C” in the context of CAP)**: The applications should always have the ability to write into the key-value storage. If the user wants strong consistency, this requirement might not always be fulfilled due to the implications of the CAP theorem.
3. **Hardware heterogeneity**: We want to add new servers with different and higher capacities, seamlessly, to our cluster without changing or upgrading existing servers. Our system should be able to accommodate and leverage different capacity servers, ensuring correct core functionality (`get` and `put` data) while balancing the workload distribution according to each server’s capacity. This calls for a peer-to-peer design with no distinguished nodes.

### Non-functional requirements

1. **Scalability**: Key-value stores should run on tens of thousands of servers distributed across the globe. Incremental scalability is highly desirable. We should add or remove the servers as needed with minimal to no disruption to the service availability. Moreover, our system should be able to handle an enormous number of users of the key-value store.
2. **Fault tolerance**: The key-value store should operate uninterrupted despite failures in servers or their components.
## Assumptions

We’ll assume the following to keep our design simple:

- The data centers hosting the service are trusted (non-hostile).
- All the required authentication and authorization are already completed.
- User requests and responses are relayed over HTTPS.
## API design

Key-value stores, like ordinary hash tables, provide two primary functions, which are `get` and `put`.
### Data type

> Dynamo uses MD5 hashes on the key to generate a 128-bit identifier. These identifiers help the system determine which server node will be responsible for this specific key.

## Scalability => [Consistent Hashing](../1.%20Base/1.%20Concepts/Consistent%20Hashing.md)

## Availability => Data [Replication](../../3.%20Database/OTLP/SQL/5.%20Distributed/Replication/Base.md)

! Peer-to-peer approach

! Usually, it’s inefficient and costly to replicate in all `n` nodes. Instead, three or five is a common choice for the number of storage nodes to be replicated.
## Versioning Data

To handle inconsistency after network break we can:
1. use the timestamps and update all conflicting values with the value of the latest request. But time isn’t reliable in a distributed system, so we can’t use it as a deciding factor;
2. use vector clock. A **vector clock** is a list of (node, counter) pairs. There’s ***a single vector clock for every version of an object***. If two objects have different vector clocks, we’re able to tell whether they’re causally related or not (more on this in a bit). Unless one of the two changes is reconciled, the two are deemed at odds.
### Modify the API design

```txt
put(key, context, value)
```

`context` - holds the metadata for each object.

To update an object in the key-value store, the client must give the `context`. We determine version information using a vector clock by supplying the `context` from a previous read operation.

## Configurable service

Use [Coordinator](../1.%20Base/2.%20Components/Coordinator.md)

Let’s take an example. Say `n` in the top `n` of the preference list is equal to 3. This means three copies of the data need to be maintained. We assume that nodes are placed in a ring. Say A, B, C, D, and E is the clockwise order of the nodes in that ring. If the write function is performed on node A, then the copies of that data will be placed on B and C. This is because B and C are the next nodes we find while moving in a clockwise direction of the ring.

Now, consider two variables, `r` and `w`. The `r` means the minimum number of nodes that need to be part of a successful read operation, while `w` is the minimum number of nodes involved in a successful write operation. So if `r=2`, it means our system will read from two nodes when we have data stored in three nodes. We need to pick values of `r` and `w` such that at least one node is common between them. This ensures that readers could get the latest-written value. For that, we’ll use a quorum-like system by setting `r+w>n`.

![](../../../_Attachments/Pasted%20image%2020240120135153.png)
Let’s say `n=3`, which means we have three nodes where the data is copied to. Now, for `w=2`, the operation makes sure to write in two nodes to make this request successful. For the third node, the data is updated asynchronously.

The coordinator produces the vector clock for the new version and writes the new version locally upon receiving a `put()` request for a key. The coordinator sends `n` highest-ranking nodes with the updated version and a new vector clock. We consider a write successful if at least `w−1` nodes respond. Remember that the coordinator writes to itself first, so we get `w` writes in total.

Requests for a `get()` operation are made to the `n` highest-ranked reachable nodes in a preference list for a key. They wait for `r` answers before returning the results to the client. Coordinators return all dataset versions that they regard as unrelated if they get several datasets from the same source (divergent histories that need reconciliation). The conflicting versions are then merged, and the resulting key’s value is rewritten to override the previous versions.
## Enable Fault Tolerance and Failure Detection

### [Hinted handoff](../1.%20Base/1.%20Concepts/Hinted%20handoff.md)

### Handle permanent failures

We need to speed up the detection of inconsistencies between replicas and reduce the quantity of transferred data.

The [[Merkle Tree]] is a mechanism to implement anti-entropy, which means to keep all the data consistent. It reduces data transmission for synchronization and the number of discs accessed during the anti-entropy process.

# References:

1. [Dynamo: Amazon’s Highly Available Key-value Store](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/decandia07dynamo.pdf)
2. [Building and operating a pretty big storage system called S3](https://www.allthingsdistributed.com/2023/07/building-and-operating-a-pretty-big-storage-system.html)
3. [**Реверс-инжиниринг архитектуры Amazon S3**](https://www.youtube.com/watch?v=O0iIADHgBVc)
4. [S3 system design | cloud storage system design | Distributed cloud storage system design](https://www.youtube.com/watch?v=UmWtcgC96X8&list=PLkQkbY7JNJuBoTemzQfjym0sqbOHt5fnV&index=29) (video)
5. [OK S3: Строим Систему Сами / Вадим Цесько (Одноклассники)](https://www.youtube.com/watch?v=N3mbocqCtsk) (video)


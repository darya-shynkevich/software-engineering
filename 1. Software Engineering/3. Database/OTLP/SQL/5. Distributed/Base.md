There are two common ways data is distributed across multiple nodes:
1. [Replication](Replication/Base.md): keeping a copy of the same data on several different nodes, potentially in different locations. Replication provides redundancy: if some nodes are unavailable, the data can still be served from the remaining nodes. Replication can also help improve performance.
2. [Sharding](Sharding/Sharding.md): splitting a big database into smaller subsets called partitions so that different partitions can be assigned to different nodes.

A database split into two partitions, with two replicas per partition:
![](../../../../../_Attachments/Pasted%20image%2020240125132603.png)

# References:

1. [An Overview of Distributed PostgreSQL Architectures](https://www.crunchydata.com/blog/an-overview-of-distributed-postgresql-architectures)
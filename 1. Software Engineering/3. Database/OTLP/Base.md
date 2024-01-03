*are typically user-facing, which means that they may see a huge volume of requests. In order to handle the load, applications usually only touch a small number of records in each query. Disk seek time is often the bottleneck here.*

Some insights about [Base](../Internals/Base.md) (HOW DB works under the hood)

- The[ log-structured school](%20log-structured%20school), which only permits appending to files and deleting obsolete files, but never updates a file that has been written. Bitcask, SSTables, LSM-trees, LevelDB, Cassandra, HBase, Lucene, and others belong to this group.
    
- The [update-in-place school](update-in-place%20school), which treats the disk as a set of fixed-size pages that can be overwritten. B-trees are the biggest example of this philosophy, being used in all major relational databases and also many nonrelational ones.

![Pasted image 20230605130337](../../../_Attachments/Pasted%20image%2020230605130337.png)

*[_Base](../Indexes/_Base.md) are a data structure that helps decrease the look-up time of requested data. Indexes achieve this with the additional costs of storage, memory, and keeping it up to date (slower writes), which allows us to skip the tedious task of checking every table row.*

*A **[Base](../Transactions/Base.md) is a unit of work you want to treat as a single unit**. Therefore, it has to either happen in full or not at all. I would argue most systems don't need to manage transactions manually, but there are situations where the increased flexibility is instrumental in achieving the desired effect. Transactions mainly deal with the **I** in **ACID,** Isolation.*

Simply put, *[Partitioning](../Distributing/Partitioning.md)* is a method for distributing data across multiple machines==*. Sharding becomes especially handy when no single machine can handle the expected workload.

**[Document database](Document%20database)** target use cases where data comes in self-constrained documents and relationships between one document and another are rare. 

**[Graph databases](Graph%20databases)** go in the opposite direction, targeting use cases where anything is potentially related to everything. 

References:
1. https://architecturenotes.co/things-you-should-know-about-databases/
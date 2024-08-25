Achieves performance only slightly below that of snapshot isolation, and significantly outperforms the traditional two-phase locking approach on read-intensive workloads.

**SSI runs transactions using snapshot isolation, but checks at runtime for conflicts between concurrent transactions, and aborts transactions when anomalies are possible.** We extended SSI to improve performance for read-only transactions, an important part of many workloads

PostgreSQL 9.1’s serializable isolation level is effective: it provides true serializability but allows more concurrency than two phase locking.



# References:

1. ~~[Serializable Snapshot Isolation in PostgreSQL](https://drkp.net/papers/ssi-vldb12.pdf)~~
With multiple copies of the same data, we are faced with options on how to synchronize them so clients have a consistent view of the data. Recall the definition of consistency from the [CAP theorem](https://github.com/donnemartin/system-design-primer#cap-theorem) - Every read receives the most recent write or an error.

### 1. **Weak consistency**

After a write, reads may or may not see it. A best effort approach is taken.
This approach is seen in systems such as memcached. Weak consistency works well in real time use cases such as VoIP, video chat, and realtime multiplayer games. For example, if you are on a phone call and lose reception for a few seconds, when you regain connection you do not hear what was spoken during connection loss.

### 2. **[Eventual Consistency](Eventual%20Consistency)**

After a write, reads will eventually see it (typically within milliseconds). Data is replicated asynchronously.
This approach is seen in systems such as DNS and email. Eventual consistency works well in highly available systems.

### 3. **Strong consistency**

After a write, reads will see it. Data is replicated synchronously.
This approach is seen in file systems and RDBMSes. Strong consistency works well in systems that need transactions.

# Important topics:

### 1. [Consistent Hashing](../1.%20The%20Introduction%20of%20System%20Design/1.%20Concepts/Consistent%20Hashing.md)

### 2. [Distributed Locks](../1.%20The%20Introduction%20of%20System%20Design/1.%20Concepts/Distributed%20Locks.md)

### 3. [Distributed Transactions](../../3.%20Database/OTLP/5.%20Distributed/Distributed%20Transactions.md)

# References:

1. **[Consistency Guarantees in Distributed Systems Explained Simply](https://kousiknath.medium.com/consistency-guarantees-in-distributed-systems-explained-simply-720caa034116)**
2. [Data Consistency and Tradeoffs in Distributed Systems](https://www.youtube.com/watch?v=m4q7VkgDWrM&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=30) (video)
3. [DBMS Consistency - (Explained By Example)](https://www.youtube.com/watch?v=Dxdh7w-0MsY&list=PLQnljOFTspQXOkIpdwjsMlVqkIffdqZ2K&index=77) (video)
4. [Distributed Consensus and Data Replication strategies on the server](https://www.youtube.com/watch?v=GeGxgmPTe4c&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=22) (video)
5. [Consistency, Availability, and Convergence](https://www.cs.utexas.edu/users/dahlin/papers/cac-tr.pdf) - Proves the upper bound for consistency possible in a typical system
6. [Consistency and Availability](https://www.infoq.com/news/2008/01/consistency-vs-availability) - Vogels
7. [Eventual Consistency](https://www.allthingsdistributed.com/2007/12/eventually_consistent.html) - Vogels
8. [Bigtable: A Distributed Storage System for Structured Data](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/chang06bigtable.pdf)
9. [If you have too much data, then 'good enough' is good enough](https://queue.acm.org/detail.cfm?id=1988603) - NoSQL, Future of data theory - Pat Helland
10. [Starbucks doesn't do two phase commit](https://www.enterpriseintegrationpatterns.com/docs/IEEE_Software_Design_2PC.pdf) - Asynchronous mechanisms at work
11. [Implementing a Transactional Outbox Pattern with DynamoDB Streams to Avoid 2-phase Commits](https://medium.com/ssense-tech/implementing-a-transactional-outbox-pattern-with-dynamodb-streams-to-avoid-2-phase-commits-ed0f91e69e9)
12. [You Can't Sacrifice Partition Tolerance](https://codahale.com/you-cant-sacrifice-partition-tolerance/) - Additional CAP commentary
13. [Решение проблемы консистентности распределенных данных в микросервисах для Python-проектов](https://www.youtube.com/watch?v=awbS6tKu1ys)
14. [Согласованность данных: что это на самом деле такое и почему с ней все так сложно](https://habr.com/ru/companies/vk/articles/723734/)
15. [Консистентность в конкуретной среде: как не захлебнуться в потоках данных](https://habr.com/ru/companies/tochka/articles/725722/)
16. [Целостность данных в микросервисной архитектуре](https://www.youtube.com/watch?v=6HvSpqBc8fA) (video)
17. [Консистентность данных в конкурентной среде. Опыт Точки](https://habr.com/ru/companies/tochka/articles/706726/)
18. [Spanner: Google’s Globally-Distributed Database](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/39966.pdf)
19. [«Отложенные данные» — наш механизм обеспечения консистентности](https://www.youtube.com/watch?v=LaShwTfAFms&list=PLH-XmS0lSi_wRIh4RJjnTGMKaTiQoaGTc&index=23) (video)
20. [Проблемы согласованности моделей в микросервисной архитектуре](https://www.youtube.com/watch?v=Ixfulg4ZFX4&list=PLH-XmS0lSi_xQtVkWsUMSVUScK_3G_LUP&index=49) (video)
21. [Как снять бэкап в распределенной системе, чтобы этого никто не заметил](https://www.youtube.com/watch?v=Zjs1B72PkD8&list=PLH-XmS0lSi_wRIh4RJjnTGMKaTiQoaGTc&index=49) (video)
22. [The Probability of Data Loss in Large Clusters](https://martin.kleppmann.com/2017/01/26/data-loss-in-large-clusters.html)
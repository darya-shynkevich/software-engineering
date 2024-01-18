It’s the fact that the various nodes of a distributed system try to reach an agreement on a specific thing.

- In the case of a distributed transaction, it’s whether a transaction has been committed or not.
- In the case of message delivery, it’s whether a message has been delivered or not.

=> **consensus problem**

![](../../../../../_Attachments/Pasted%20image%2020240118154143.png)

# [[Leader election]]

# [Distributed Locks](../Distributed%20Locks.md)

# [Atomic broadcast](../Atomic%20broadcast.md)

# FLP impossibility theory

It’s proven that in _asynchronous_ systems, where there can be at least one faulty node, any possible consensus algorithm will be unable to terminate under some scenarios. In other words, there can be no consensus algorithm that will always satisfy all the aforementioned properties. This is referred to as the **FLP impossibility** named after the last initials of the authors of the associated paper:
- The fact that it’s always possible that the system’s initial state is one where nodes can reach different decisions depending on the ordering of messages (the so-called bivalent configuration), as long as there can be at least one faulty node
- From such a state, it’s always possible to end up in another bivalent state, just by introducing delays in some messages

# Solutions

Can not be coordinated by one node, because the failure of a single node (the _coordinator_) could bring the whole system to a halt.
## [Paxos](Paxos.md) Algoritm



# References:

1. [A Beginner’s Guide to Consensus Algorithms](https://medium.com/@abhishekranjandev/a-beginners-guide-to-consensus-algorithms-148f68452a6a)
2. [In Search of an Understandable Consensus Algorithm](https://raft.github.io/raft.pdf) - The extended version of the RAFT paper, an alternative to PAXOS.
3. [Mencius: Building Efficient Replicated State Machines for WANs](https://www.usenix.org/legacy/event/osdi08/tech/full_papers/mao/mao_html/) - consensus algorithm for wide-area network


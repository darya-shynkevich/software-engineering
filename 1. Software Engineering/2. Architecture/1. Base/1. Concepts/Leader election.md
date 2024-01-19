It is a widespread problem where the nodes that are part of a distributed system need to elect one node from amongst them to act as their leader, and coordinate the operation of the whole system.

**Example:** *An example of this problem is the **single-master replication** scheme.* *This scheme is based on the fact that one node, designated as primary, will be responsible for performing operations that update data. The other nodes, designated as secondaries, will follow up with the same operations.* *However, the system first needs to select the primary node through a process called **leader election**. Since all the nodes are practically agreeing on a single value, the identity of the leader, this problem can easily be modeled as a **consensus problem**.*




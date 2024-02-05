A **quorum** is the minimum number of members that must be present at any meeting to consider the proceedings of the meeting valid.

Quorum is a widely used concept in distributed systems. For many of the standard algorithms, the idea of quorum is used to reach a decision.

Let’s understand the concept of quorum with the help of a diagram.

![](../../../../_Attachments/Pasted%20image%2020240119232443.png)

In Fig A, we have two nodes, each with green and red colours. In this case, there can be no consensus, as a quorum of nodes has not agreed. On the other hand, in Fig B, we have three green nodes, and hence we have a quorum. Whatever these three nodes agree upon is what the algorithm will do.

In distributed systems, we mostly have an odd number of nodes, so the chances of us running into a situation like the one depicted in Fig A are slim. There will always be more than half the number of given nodes deciding on something, provided that there are two options to choose from.

Quorum is used in almost all consensus-based algorithms in distributed systems. [[../../1. Concepts/Consensus Algorithms/Raft]] is one of the famous consensus algorithms that uses quorum as the core of their algorithm logic.

# Problem of split brain in distributed systems

Network partition is an inherent part of any distributed system, and a system that claims to be distributed also needs to be partition tolerant. That is, in the event of a network partition, the system should continue to work.

Quorum usually helps with this continuity in the event of a network partition. In the absence of a quorum, the different partitions would each assume that the view of the system is as they see it, which is limited by the communication. Each of the partitions would then continue to work and move forward in different directions. This problem is known as **split brain**.

With the concept of quorum, the system would only work if more than half the nodes can communicate with each other through some network path.

# DB replication example

Let’s suppose we have three nodes. If at least two out of three nodes are guaranteed to return successful updates, it means only one node has failed. This means that if we read from two nodes, at least one of them will have the updated version, and our system can continue working.

If we have `n` nodes, then every write must be updated in at least `w` nodes to be considered a success, and we must read from `r` nodes. We’ll get an updated value from reading as long as `w+r>n` because at least one of the nodes must have an updated write from which we can read. Quorum reads and writes adhere to these `r` and `w` values. These `n`, `w`, and `r` are configurable in Dynamo-style databases.

![](../../../../_Attachments/Pasted%20image%2020240119195425.png)

# References:

1. https://architecturenotes.co/quorums/ 
2. ~~[What is a quorum?](https://www.educative.io/answers/what-is-a-quorum)~~
3. ~~[What is quorum in distributed systems?](https://www.educative.io/answers/what-is-quorum-in-distributed-systems)~~
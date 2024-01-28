(+) can be useful in applications in which we can continue work offline and later sync our data
(+) better performance and scalability than single leader replication

(-) conflicts when the same data changes

As discussed above, single leader replication using asynchronous replication has a drawback. There’s only one primary node, and all the writes have to go through it, which limits the performance. In case of failure of the primary node, the secondary nodes may not have the updated database.

**Multi-leader replication** is an alternative to single leader replication. ***There are multiple primary nodes that process the writes and send them to all other primary and secondary nodes to replicate.*** 

**When useful:**
1. **Multi-datacenter operation:** with a normal leader-based replication setup, the leader has to be in one of the datacenters, and all writes must go through that datacenter
2. Clients with offline operation: application that needs to continue to work while it is disconnected from the internet. In this case, every device has a local database that acts as a leader (it accepts write requests), and there is an asynchronous multi-leader replication process (sync) between the replicas of your calendar on all of your devices. The replication lag may be hours or even days, depending on when you have internet access available.
3. Collaborative editing

! This type of replication is used in databases along with external tools like the [[Tungsten Replicator]] for MySQL. BDR for PostgreSQL and GoldenGate for Oracle

![](../../../../../../../_Attachments/Pasted%20image%2020240119194307.png)

As multi-leader replication is a somewhat retrofitted feature in many databases, there are often subtle configuration pitfalls and surprising interactions with other database features. For example, autoincrementing keys, triggers, and integrity constraints can be problematic. For this reason, **multi-leader replication is often considered dangerous territory that should be avoided if possible**.

## Handling Write Conflicts

If you want **synchronous conflict detection**, you might as well just use single-leader replication.
### 1. Conflict avoidance

Since many implementations of multi-leader replication handle conflicts quite poorly, avoiding conflicts is a **frequently recommended approach**.

For example, in an application where a user can edit their own data, you can ensure that requests from a particular user are always routed to the same datacenter and use the leader in that datacenter for reading and writing. Different users may have different “home” datacenters (perhaps picked based on geographic proximity to the user), but from any one user’s point of view the configuration is essentially single-leader.

However, sometimes you might want to change the designated leader for a record— perhaps because one datacenter has failed and you need to reroute traffic to another datacenter, or perhaps because a user has moved to a different location and is now closer to a different datacenter. In this situation, conflict avoidance breaks down, and you have to deal with the possibility of concurrent writes on different leaders.
### 2. Converging toward a consistent state

- Give each write a unique ID (e.g., a timestamp, a long random number, a UUID, or a hash of the key and value), pick the write with the highest ID as the winner, and throw away the other writes. If a timestamp is used, this technique is known as last write wins (**LWW**). Although this approach is popular, **it is dangerously prone to data loss**.
- Give each replica a unique ID, and let writes that originated at a higher-numbered replica always take precedence over writes that originated at a lower-numbered replica. This approach also implies data loss.
- Somehow **merge the values together** — e.g., order them alphabetically and then concatenate them.
- Record the conflict in an explicit data structure that preserves all information, and write application code that resolves the conflict at some later time (perhaps by prompting the user).
### 3. Custom conflict resolution logic

As the most appropriate way of resolving a conflict may depend on the application, most multi-leader replication tools let you write conflict resolution logic using appli‐ cation code. That code may be executed on write or on read:
1. On write: As soon as the database system detects a conflict in the log of replicated changes, it calls the conflict handler.
2. On read: When a conflict is detected, all the conflicting writes are stored. The next time the data is read, these multiple versions of the data are returned to the application. The application may prompt the user or automatically resolve the conflict, and write the result back to the database.

## Multi-Leader Replication Topologies

![](../../../../../../../_Attachments/Pasted%20image%2020240128161928.png)

The **most general topology is all-to-all**, in which every leader sends its writes to every other leader. However, more restricted topologies are also used: for example, **MySQL by default supports only a circular topology,** in which each node receives writes from one node and forwards those writes (plus any writes of its own) to one other node. **Another popular** topology has the shape of a **star:** one designated root node forwards writes to all of the other nodes. The star topology can be generalised to a tree.

! To prevent infinite replication loops, each node is given a unique identifier, and in the replication log, each write is tagged with the identifiers of all the nodes it has passed through. When a node receives a data change that is tagged with its own identifier, that data change is ignored, because the node knows that it has already been processed.

!! A problem with **circular and star topologies** is that if just one node fails, it can **interrupt the flow of replication messages between other nodes.** The fault tolerance of a more densely connected topology (such as all-to-all) is better because it allows messages to travel along different paths, avoiding a single point of failure.

!!! Yet there is a **problem with all-to-all replication**: some network links may be faster than others (e.g., due to network congestion), with the result that some replication messages may “overtake” others.
	=> there is a possible problem when we will try to update row, which `insert` has not been propagated to all nodes.
		=> version vectors


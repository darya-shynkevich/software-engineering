### 1. Synchronous Replication

Synchronous replication is a type of database replication where ***changes made to the primary database are immediately replicated*** to the replica databases ***before the write operation is considered complete***. In other words, the primary database waits for the replica databases to confirm that they have received and processed the changes before the write operation is acknowledged.

Example: [[Cassandra]]

![Pasted image 20231014145242](../../../../../../_Attachments/Pasted%20image%2020231014145242.png)

**Pros:**
- Consistency: Since replicas update instantly, all databases remain in harmony, eliminating data mismatches.
- Immediate Feedback: Any issue during data transfer is instantly known, allowing swift corrections.

**Cons:**
- Latency Issues: Waiting for confirmation from the replica can introduce delays, affecting performance.
- Dependency: If a replica is unreachable, the whole update process can stall.

**Ideal For**: Systems where data integrity and consistency are paramount, such as financial and medical databases.
### 2. Asynchronous Replication

Asynchronous replication is a type of database replication where ***changes made to the primary database are not immediately replicated to the replica databases***. Instead, the ***changes are queued and replicated to the replicas at a later time***.

![Pasted image 20231014145400](../../../../../../_Attachments/Pasted%20image%2020231014145400.png)

**Pros:**
- Performance Boost: Without waiting for confirmations, operations are faster.
- Resilience: Even if a replica is down, the primary continues functioning without hitches.

**Cons:**
- Data Lag: Replicas might not always have the latest data immediately.
- Potential Data Loss: If the primary crashes before a replica updates, that recent data might be lost.

**Ideal For**: Web applications where speed is crucial and minor data discrepancies can be tolerated.
### 3. **Semi-synchronous Replication**

Semi-synchronous replication is a type of database replication that combines elements of both synchronous and asynchronous replication. In semi-synchronous replication, ***changes made to the primary database are immediately replicated to at least one replica database, while other replicas may be updated asynchronously.***

In semi-synchronous replication, the write operation on the primary is not considered complete until ***at least one replica database has confirmed that it has received and processed the changes.*** This ensures that there is some level of strong consistency between the primary and replica databases, while also providing improved performance compared to fully synchronous replication.

![Pasted image 20231014145528](../../../../../../_Attachments/Pasted%20image%2020231014145528.png)

**Pros:**
- Balanced Approach: It combines the speed of asynchronous with the reliability of synchronous replication.
- Guaranteed Backup: At least one replica is always up-to-date, ensuring data safety.

**Cons:**
- Partial Delays: While faster than fully synchronous, waiting for at least one confirmation can introduce some lag.
- Complexity: Implementing and maintaining this hybrid can be more intricate.

**Ideal For**: E-commerce platforms where a balance of speed and data safety is necessary.
### 4. Chain Replication

Used by Microsoft Azure Storage.

## Factors to Consider When Choosing a Replication Strategy

### 1. **Data Consistency Requirements**

**What’s At Stake?**: How crucial is it for your replicas to mirror the primary database in real-time?

**Example**: In financial systems, where a slight inconsistency can lead to significant discrepancies, immediate consistency is non-negotiable. On the other hand, a blogging platform can afford a slight lag between the primary and replicas.

### 2.  **Write Performance and Latency**

**What’s At Stake?**: Speed! Do you prioritize rapid write operations, or can you afford a slight delay for data integrity?

**Example**: A real-time gaming platform cannot afford latency in updates. Here, the speed of writing data (even if it compromises immediate consistency) can be more critical than in a digital library, where a minor delay in updating a catalog might be acceptable.

### 3.  **==Network Infrastructure and Bandwidth==**

**What’s At Stake?**: The physical aspects of your system. Can your network handle constant communication between primary and replicas?

**Example**: *==In regions with unreliable or slow network connections, asynchronous replication might be more feasible than synchronous, which requires constant and speedy communication.==*

### 4.  **Geographical Distribution of Databases**

**What’s At Stake?**: The locations of your databases and their distances from one another.

**Example**: If you have a globally dispersed user base, it might make sense to have replicas in various continents. However, *==the farther apart these databases are, the higher the latency, which might push you towards an asynchronous or semi-synchronous approach.==*

### 5. **Recovery and Failover Strategies**

**What’s At Stake?**: How quickly and smoothly can your system recover from failures?

**Example**: E-commerce platforms that see heavy traffic, especially during sales, need robust recovery strategies. If the primary server fails, a replica needs to take over swiftly to avoid revenue losses. Here, the strategy should align with quick failover capabilities.

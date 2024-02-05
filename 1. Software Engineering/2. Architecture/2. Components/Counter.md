
In social media application there is a problem with a popular posts that have a lot of likes (a lot of write operations). 

The challenge is that writing takes more time than reading, and **concurrent** activity makes this problem harder. As the number of concurrent writes increases for some counter (which might be a variable residing in a node’s memory), the **lock contention** increases non-linearly. After some point, we might spend most of the time acquiring the lock so that we could safely update the counter.

**Solutions**
1. Concurrent counter => inconsistencies in the data
2. Store updates in queue => expense of added delay
3. Sharded counter = distributed counter (each counter has a specified number of shards as needed)

A write request is forwarded to the specified **tweet counter** when the user likes that tweet. Then, the system chooses an available shard of the specified tweet counter to increment the like count.

![Pasted image 20231113144731](../../../../_Attachments/Pasted%20image%2020231113144731.png)
The **number of shards** is critical for good performance. 
If the shard count is small for a specific write workload => high write contention, which results in slow writes.
If the shard count is too high for a particular write profile, we encounter a higher overhead on the read operation.

## Manage read requests

When the user sends the read request, the system will aggregate the value of all shards of the specified counter to return the total count of the feature (such as like or reply). **Accumulating values** from all the shards on each read request **will result in low read throughput and high read latency**.

The decision of when the system will sum all shards values is also very critical. If there is high write traffic along with reads, it might be virtually **impossible** to get a real current value because by the time we report a read value to the client, it will have already changed. So, **periodically reading all the shards of a counter** and **caching** it should serve most of the use cases. By reducing the accumulation period, we can increase the accuracy of read values.

## Evaluation

**Availability**: The system remains available even if some shards go down or suffer a fault. This way, sharded counters provide high availability.

**Scalability**: Sharded counters allow high horizontal scaling as needed.

**Reliability**: 


 
# References:

1. [Sharded Counters](!https://medium.com/@sureshpodeti/sharded-counters-8a9a760a7b53)
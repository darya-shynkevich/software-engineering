Although Kafka stores data on Disk it provides high throughput at low latency due to 
1. Sequential I/O 
2. Page Cache 
3. Zero Copy Data transfer. + Memory Map 
4. Batching
5. Compression
6. Compaction
7. Parallelism Based on Topic partitions
8. No serialisation/deserialisation on the Broker

## 1. Sequential I/O

> Sequential Access in disk is faster than Random Access in memory.

Kafka uses an **append-only log** as its primary data structure. It adds new data to the end of the file thus making writes sequential.
![Pasted image 20231019233113](../../../../../_Attachments/Pasted%20image%2020231019233113.png)

**Drawbacks:**
1. ***Lack of Random Access => Slow Retrieval of Specific Data***: One major drawback is the inability to directly access specific data points within a sequence
2. ***Limited Parallelism:*** Sequential I/O restricts parallel processing capabilities. Since data is accessed in a linear manner, it becomes challenging to perform multiple read or write operations simultaneously.

## 2. Utilizing Linux Page cache

> Page Cache is used to Cache file data on the file system, when a process is performing read/write operations on the file.

Kafka’s data is **not written to the hard disk in real-time.** When the Broker receives data, it writes the data to the Page Cache first and later on flushes it to disk asynchronously. 
Writing to Page cache has the following advantages:
1. The I/O scheduler batches together consecutive small logical writes into bigger physical writes which improves throughput
2. The I/O scheduler attempts to re-sequence writes to minimize the movement of the disk head which improves throughput
3. It automatically uses all the free memory (non-JVM memory) on the machine.
4. If the consumption and production rates are comparable, data does not even need to be exchanged through physical disks and can be directly read through the Page Cache.

## 3. Zero Copy Data Transfer

There are two ways to achieve Zero Copy Data transfer linked to Kafka.

1. `sendFile` + DMA  
2. `mmap` + write

Since Kafka keeps the data in the same (binary) format during its lifecycle, it does not need to load the data in the application buffer, it directly copies data from the page cache to the NIC buffer. `sendfile` system call of Linux reduces byte copying (across kernel and user spaces) and context switches, hence making the process faster. This copy uses DMA (direct memory access) which means that the CPU is not involved and that makes the process way more efficient (bringing down the time by ~60 % ).

![](../../../../../_Attachments/Pasted%20image%2020240121164425.png)
Memory Mapped Files — The operations on memory are reflected in disk files.

Mmap maps the read buffer in Kernel to user buffer in user space. This process eliminates the need of copying the data from kernal to user buffer. Mmap improves I/O for large files. **Kafka uses Memory Mapped Files for index and timeindex.**

> _Note: When SSL is enabled, zero-copy optimization is lost, since the Broker needs to decrypt and encrypt the data._

## 4. Batching

Kafka clients and brokers will accumulate multiple records in a batch — for both reading and writing — before sending them over the network. Batching of records amortises the overhead of the network round-trip, using larger packets and improving bandwidth efficiency.

![](../../../../../_Attachments/Pasted%20image%2020240121164554.png)
## 5.  Compression

The Producer compresses data and sends it to the broker to reduce the cost of network transmission. Currently, the supported compression algorithms include `Snappy`, `Gzip`, and `LZ4`. Data compression is often used in conjunction with batch processing as an optimisation tool.
![](../../../../../_Attachments/Pasted%20image%2020240121164653.png)
## 6. Compaction

Log compaction ensures that Kafka will only retain the latest known value for each message key within the log of data for a single topic partition.
![](../../../../../_Attachments/Pasted%20image%2020240121164733.png)

## 7. Parallelism Based on Topic partitions

Every partition has a dedicated leader hence any nontrivial topic (with multiple partitions) can, therefore, utilize the entire cluster of brokers for writes and reads. Consumers also can consume messages from any partition which resides in any of the brokers hence efficiently using the whole cluster. We can even configure different partitions on the same node to reside on different disks. In this way, we can take advantage of the parallel processing of multiple disks.
![](../../../../../_Attachments/Pasted%20image%2020240121164828.png)
## 8. No serialisation/deserialisation on the Broker

A significant amount of work is performed on the client side before records get to the server/broker. This includes the staging of records in an accumulator, hashing the record keys to arrive at the correct partition index, checksumming the records and the compression of the record batch. The client is aware of the cluster metadata and periodically refreshes this metadata to keep abreast of any changes to the broker topology. This lets the client make low-level forwarding decisions; rather than sending a record blindly to the cluster and relying on the latter to forward it to the appropriate broker node, a producer client will forward writes directly to partition masters. Similarly, consumer clients are able to make intelligent decisions when sourcing records, potentially using replicas that are geographically closer to the client when issuing read queries.

# References:

1. [What makes Kafka so performant?](https://blog.devgenius.io/what-makes-kafka-so-performant-df5dbecb7f3a)
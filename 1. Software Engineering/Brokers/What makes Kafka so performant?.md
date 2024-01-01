Although Kafka stores data on Disk it provides high throughput at low latency due to 
1. Sequential I/O 
2. Page Cache 
3. Memory Map 
4. Zero Copy Data transfer.

## 1. Sequential I/O

> Sequential Access in disk is faster than Random Access in memory.

Kafka uses an **append-only log** as its primary data structure. It adds new data to the end of the file thus making writes sequential.
![Pasted image 20231019233113](../../_Attachments/Pasted%20image%2020231019233113.png)

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


# References:

1. [What makes Kafka so performant?](https://blog.devgenius.io/what-makes-kafka-so-performant-df5dbecb7f3a)
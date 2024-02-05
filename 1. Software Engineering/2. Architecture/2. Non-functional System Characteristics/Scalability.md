**Scalability** is the ability of a system to handle an increasing amount of workload without compromising performance.

Load can be described with a few numbers which we call ***load parameters***. The best choice of parameters depends on the architecture of your system: it may be ***requests per second*** to a web server, the ***ratio of reads to writes in a database***, the ***number of simultaneously active users*** in a chat room, the ***hit rate on a cache***, or something else.

- **Request workload**: This is the number of requests served by the system.
- **Data/storage workload**: This is the amount of data stored by the system.

## Different approaches of scalability

### 1. Vertical scalability—scaling up

**Vertical scaling**, also known as “**scaling up**,” refers to scaling by providing additional capabilities (for example, additional CPUs or RAM) to an existing device. Vertical scaling allows us to expand our present hardware or software capacity, but we can only grow it to the limitations of our server. The dollar cost of vertical scaling is usually high because we might need exotic components to scale up.

### 2. Horizontal scalability—scaling out

**Horizontal scaling**, also known as “**scaling out**,” refers to increasing the number of machines in the network. We use commodity nodes for this purpose because of their attractive dollar-cost benefits. The catch here is that we need to build a system such that many nodes could collectively work as if we had a single, huge server.

## Example: Twitter Timelines at Scale

![Pasted image 20230628184059](../../../_Attachments/Pasted%20image%2020230628184059.png)
*The first version of Twitter used approach 1, but the systems struggled to keep up with the load of home timeline queries, so the company switched to approach 2. This works better because the average rate of published tweets is almost two orders of magnitude lower than the rate of home timeline reads, and so in this case it’s preferable to do more work at write time and less at read time.*

*However, the downside of approach 2 is that posting a tweet now requires a lot of extra work. On average, a tweet is delivered to about 75 followers, so 4.6k tweets per second become 345k writes per second to the home timeline caches.*

*The final twist of the Twitter anecdote: now that approach 2 is robustly implemented, Twitter is moving to a hybrid of both approaches. Most users’ tweets continue to be fanned out to home timelines at the time when they are posted, but a small number of users with a very large number of followers (i.e., celebrities) are excepted from this fan-out. Tweets from any celebrities that a user may follow are fetched separately and merged with that user’s home timeline when it is read, like in approach 1. This hybrid approach is able to deliver consistently good performance.*

## Describing Performance

***Latency*** and ***response time*** are often used synonymously, but they are not the same. *The response time is what the client sees*: besides the actual time to process the request (the service time), it includes network delays and queueing delays. *Latency is the duration that a request is waiting to be handled — during which it is latent, awaiting service*

Random additional latency could be introduced by a context switch to a background process, the loss of a network packet and TCP retransmission, a garbage collection pause, a page fault forcing a read from disk, mechanical vibrations in the server rack, or many other causes.

***High percentiles*** of response times, also known as ***tail latencies***, are important because they directly affect users’ experience of the service. Amazon has also observed that a 100 ms increase in response time reduces sales by 1%, and others report that a 1-second slowdown reduces a customer satisfaction metric by 16%.

*On the other hand, optimizing the 99.99th percentile (the slowest 1 in 10,000 requests) was deemed too expensive and to not yield enough benefit for Amazon’s purposes. Reducing response times at very high percentiles is difficult because they are easily affected by random events outside of your control, and the benefits are diminishing.*

## Approaches for Coping with Load

While distributing stateless services across multiple machines is fairly straightforward, taking stateful data systems from a single node to a distributed setup can introduce a lot of additional complexity. For this reason, *common wisdom until recently was to keep your database on a single node (scale up) until scaling cost or high- availability requirements forced you to make it distributed.*

The architecture of systems that operate at large scale is usually highly specific to the application — *there is no such thing as a generic, one-size-fits-all scalable architecture* (informally known as ***magic scaling sauce***). The problem may be the volume of reads, the volume of writes, the volume of data to store, the complexity of the data, the response time requirements, the access patterns, or (usually) some mixture of all of these plus many more issues.

# Case studies

1. [What is Scalability Anyway?](https://brooker.co.za/blog/2024/01/18/scalability.html)
2. [10 Core Architecture Pattern Variations For Achieving Scalability](http://highscalability.com/blog/2011/11/7/10-core-architecture-pattern-variations-for-achieving-scalab.html)
3. [Discord](../Examples/Discord.md)
4. [When Taylor Swift crashed Ticketmaster: A lesson on scaling for spikes](https://learningdaily.dev/when-taylor-swift-crashed-ticketmaster-a-lesson-on-scaling-for-spikes-9931e2c888e9)
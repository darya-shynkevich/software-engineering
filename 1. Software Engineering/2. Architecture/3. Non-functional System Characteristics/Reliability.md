1. The application performs the function that the user expected.
2. It can tolerate the user making mistakes or using the software in unexpected ways.
3. Its performance is good enough for the required use case, under the expected load and data volume.
4. The system prevents any unauthorized access and abuse.

We often use **mean time between failures (MTBF)** and **mean time to repair (MTTR)** as metrics to measure `R`.
![](../../../_Attachments/Pasted%20image%2020240118163818.png)
(We strive for a higher MTBF value and a lower MTTR value.)

> There are many variations of the MTBF metric, such as mean time to failure (MTTF). 
> Usually, we use MTTF (Mean Time To Failure) instead of MTBF for those cases where a failed component is replaced due to irreparable problems. A bad disk or a failed bulb are examples of irreparable faults where a replacement is required.

***Reliability means making systems work correctly, even when faults occur.*** Faults can be in hardware (typically random and uncorrelated), software (bugs are typically systematic and hard to deal with), and humans (who inevitably make mistakes from time to time). Fault-tolerance techniques can hide certain types of faults from the end user.

The things that can go wrong are called **faults**, and systems that anticipate faults and can cope with them are called **fault-tolerant** or **resilient**.

# Reliability and [[CAP/Availability]]

Reliability and availability are two important metrics to measure compliance of service to agreed-upon service level objectives (SLO).

The ***measurement of availability is driven by time loss***, whereas the ***frequency and impact of failures drive the measure of reliability***. 

! Availability and reliability are essential because they enable the stakeholders to assess the health of the service.

1. **Reliability** measures how well a system performs its intended operations (functional requirements). We use averages for that (Mean Time to Failure, Mean Time to Repair, etc.)
2. **Availability** measures the percentage of time a system accepts requests and responds to clients.

**Example 1:** A certain system may be 90% available but only reliable 80% of the time.

**Example 2:** Suppose we consider our “system” the stuff inside a data center (hardware + software). Let’s assume this data center suffers a network failure such that no outsider traffic is coming in and no insider traffic is going out. In this case, instantaneous availability might be zero (because clients cannot reach the service) even though inside the data center, all systems are perfectly functioning (instantaneous reliability 100%).

We use both of them (reliability and availability) in different contexts. For example, storage vendors often quote MTTF for their disks. Most online services use uptime (as a measure of availability) in their SLAs. For example, the uptime of EC2 virtual machines is 99.95%.

# Severity 

![](../../../_Attachments/Pasted%20image%2020240118162814.png)

## 1. Fail-stop

In this type of failure, a node in the distributed system halts permanently. However, the other nodes can still detect that node by communicating with it.

From the perspective of someone who builds distributed systems, fail-stop failures are the simplest and the most convenient.
## 2. Crash

In this type of failure, a node in the distributed system halts silently, and the other nodes can’t detect that the node has stopped working.
## 3. Omission failures

In **omission failures**, the node fails to send or receive messages. There are two types of omission failures. If the node fails to respond to the incoming request, it’s said to be a **send omission failure**. If the node fails to receive the request and thus can’t acknowledge it, it’s said to be a **receive omission failure**.
## 3. Temporal failures

In **temporal failures**, the node generates correct results, but is too late to be useful. This failure could be due to bad algorithms, a bad design strategy, or a loss of synchronisation between the processor clocks.
## 4. Byzantine failures

In **Byzantine failures**, the node exhibits random behaviour like transmitting arbitrary messages at arbitrary times, producing wrong results, or stopping midway. This mostly happens due to an attack by a malicious entity or a software bug. A byzantine failure is the most challenging type of failure to deal with.

# Types
### Hardware Faults

- Disks may be set up in a RAID configuration, servers may have dual power supplies and hot-swappable CPUs, and datacenters may have batteries and diesel generators for backup power
- This approach cannot completely prevent hardware problems from causing failures, but it is well understood and can often keep a machine running uninterrupted for years.

*[In some cloud platforms such as Amazon Web Services (AWS) it is fairly common for virtual machine instances to become unavailable without warning, as the platforms are designed to prioritize flexibility and elasticityi over single-machine reliability.](https://web.archive.org/web/20160429075023/http://blog.awe.sm/2012/12/18/aws-the-good-the-bad-and-the-ugly/ )*

### Software Errors

- Such faults are harder to anticipate, and because they are correlated across nodes, they tend to cause many more system failures than uncorrelated hardware faults.

### Human Errors

- Even when they have the best intentions, humans are known to be unreliable: 0. one study of large internet services found that configuration errors by operators were the leading cause of outages, whereas hard‐ ware faults (servers or network) played a role in only 10–25% of outages.

# How to?:

1. Design systems in a way that minimizes opportunities for error. For example, well-designed abstractions, APIs, and admin interfaces make it easy to do “the right thing” and discourage “the wrong thing.” However, if the interfaces are too restrictive people will work around them, negating their benefit, so this is a tricky balance to get right.
2. Decouple the places where people make the most mistakes from the places where they can cause failures. In particular, provide fully featured non-production sandbox environments where people can explore and experiment safely, using real data, without affecting real users.
3. Test thoroughly at all levels, from unit tests to whole-system integration tests and manual tests. Automated testing is widely used, well understood, and espe‐ cially valuable for covering corner cases that rarely arise in normal operation.
4. Allow quick and easy recovery from human errors, to minimize the impact in the case of a failure. For example, make it fast to roll back configuration changes, roll out new code gradually (so that any unexpected bugs affect only a small subset of users), and provide tools to recompute data (in case it turns out that the old com‐ putation was incorrect).
5. Set up detailed and clear monitoring, such as performance metrics and error rates. In other engineering disciplines this is referred to as ***telemetry***. *(Once a rocket has left the ground, telemetry is essential for tracking what is happening, and for understanding failures)* Monitoring can show us early warning signals and allow us to check whether any assumptions or constraints are being violated. When a problem occurs, metrics can be invaluable in diagnosing the issue.
6. Implement good management practices and training.
### [Troubleshooting](../../8.%20Troubleshooting/_Base.md)

### References:

1. [What Bugs Live in the Cloud?](https://ucare.cs.uchicago.edu/pdf/socc14-cbs.pdf)
2. [How Complex Systems Fail](https://www.adaptivecapacitylabs.com/HowComplexSystemsFail.pdf)
3. [Getting Real About Distributed System Reliability](https://blog.empathybox.com/post/19574936361/getting-real-about-distributed-system-reliability)
4. [Why do Internet services fail, and what can be done about it?](https://static.usenix.org/events/usits03/tech/full_papers/oppenheimer/oppenheimer.pdf)
5. [Simple Testing Can Prevent Most Critical Failures: An Analysis of Production Failures in Distributed Data-Intensive Systems](https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-yuan.pdf)
6. [The Human Impact of Bugs](http://jury.me/blog/2013/3/14/the-human-impact-of-bugs)
7. [Understanding and Addressing Common Production Failures in Systems and Applications](https://jinlow.medium.com/understanding-and-addressing-common-production-failures-in-systems-and-applications-3f045ed2c24a)
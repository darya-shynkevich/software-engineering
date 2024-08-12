![Pasted image 20230605160546](Pasted%20image%2020230605160546.png)

### 1. The network is reliable
![Pasted image 20230605221549](Pasted%20image%2020230605221549.png)
*==Any particular communication can fail==*; Therefore, we need to provide a way for systems to deal with this potential miscommunication.

*The ***store and forward*** *pattern involves storing data before sending it to downstream servers, which provides recovery options in catastrophic scenarios. There are numerous technologies, such as RabbitMQ and ActiveMQ, that follow this pattern.*

### 2. Latency is zero

Pictured on the left is the time to access memory in a modern system, on the right the time it takes to do a round trip across the world.
![Pasted image 20230605221758](Pasted%20image%2020230605221758.png)
*I like to think about latency as strictly overhead to get any request done. Message can be large, or it can be small, and latency is unchanged. Unlike bandwidth, latency usually has to do with the speed of light and the communication distance (or path). So ==the distance between the two systems plays a significant role here==.*

*==Latency is omnipresent. It occurs in all communication.==*

*Latency is very similar to unloading groceries from the car.  The time it takes you to travel  from the kitchen to the car is latency. Do you want to grab as much as you can in one trip, or do you want to bring the items individually, taking several hundred round-trips to unload the car?*

***Content delivery networks** and edge computing are essentially trying to make the distance between the fridge and trunk as close as possible. By duplicating the data closer to where it is needed we significantly reduce latency.*

### 3. Bandwidth is infinite

![Pasted image 20230605222119](Pasted%20image%2020230605222119.png)

*Increasing data size without limit can lead to scale difficulties and hitting communication channel limits. This was experienced when the payload for a homepage API was accidentally increased by a factor of 10, causing bandwidth limits to be reached and the site to go down. While taking more data on each round trip can reduce latency effects, it has limits based on system design and priorities, and being aware of trade-offs is important.*

### 4. The network is secure

*Failing to consider security when designing a system can be a big mistake. With the increase in security breaches, ==incorporating security measures at the outset is necessary==. This includes assessing current security issues and improving them. ==Taking a security-first approach can be productive in the long run==.*

### 5. Topology doesn't change
![Pasted image 20230605222500](Pasted%20image%2020230605222500.png)
*The structure of a network can change and may cause traffic to not flow to the correct destination. With tools like Docker and Kubernetes, it's easier to change network topology. ==However, it's important to consider potential failures and build systems that can be resilient to changes.== Tools like Zookeeper and Consul can help with service discovery and reacting to changes in the system's layout.*

### 6. There is one administrator

*You can't control everything in your system. As your system grows, you'll rely on other systems outside your control. It's important to have a way to manage your systems and configurations. Infrastructure as Code can help codify those variations in your systems. ==Monitoring and observability tools are important, as is appropriate decoupling to ensure system resiliency and uptime.*==

### 7. Transport cost is zero

![Pasted image 20230605222857](Pasted%20image%2020230605222857.png)

*It's important to consider the cost of sending data between systems, especially as the systems grow. ==Optimizing message formats== like gRPC or MessagePack ==can help reduce overhead and cost==. However, it's ==important to weigh the tradeoffs== because doing so early can create more problems than it solves in the short term.*

### 8. The network is homogeneous
![Pasted image 20230605223020](Pasted%20image%2020230605223020.png)
*==Solutions must be adaptable and interoperable== to function in diverse environments with differing frameworks. This flexibility prevents problems and saves time in the long run.*

# References:

1. ~~[Fallacies of Distributed Systems](https://architecturenotes.co/p/fallacies-of-distributed-systems)~~
2. [Fallacies of Distributed Computing series](https://particular.net/blog/the-network-is-reliable)
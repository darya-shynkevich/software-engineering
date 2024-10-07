A load balancer is a network device or software application that is used to evenly distribute incoming network traffic among multiple servers. Its primary purpose is to *==ensure optimal resource utilization, reduce latency, and maintain high availability of services==*. This is particularly important in scenarios where there are multiple servers that can handle incoming requests and where uneven distribution or sudden spikes in traffic can occur.

Load balancers employ various algorithms to determine how to distribute incoming traffic among the available servers.

(+) **Reduced downtime**
(+) **Availability:** if some servers go down or suffer a fault, the system still remains available. One of the jobs of the load balancers is to hide faults and failures of servers
(+) **Scalability**: by adding servers, the capacity of the application/service can be increased seamlessly. Load balancers make such upscaling or downscaling transparent to the end users.
(+) **Redundancy**
(+) **Flexibility**
(+) **Performance:** can forward requests to servers with a lesser load so the user can get a quicker response time. This not only improves performance but also improves resource utilisation.
# Placing LB

Generally, LBs sit between clients and servers. Requests go through to servers and back to clients via the load balancing layer. 

In reality, load balancers can be potentially used between any two services with multiple instances within the design of a system.
# Services offered by load balancers

1. **[Healthchecks](../../2.%20Architecture/1.%20Concepts/Healthchecks.md)** LBs use the heartbeat protocol to monitor the health and, therefore, reliability of end-servers. Another advantage of health checking is the improved user experience.
2. **TLS termination**: LBs reduce the burden on end-servers by handling TLS termination with the client.
3. **Predictive analytics**: LBs can predict traffic patterns through analytics performed over traffic passing through them or using statistics of traffic obtained over time.
4. **Reduced human intervention**: Because of LB automation, reduced system administration efforts are required in handling failures.
5. **Service discovery**: An advantage of LBs is that the clients’ requests are forwarded to appropriate hosting servers by inquiring about the service registry.
6. **Security**: LBs may also improve security by mitigating attacks like denial-of-service (DoS) at different layers of the OSI model (layers 3, 4, and 7).

As a whole, load balancers provide flexibility *(add or remove machines transparently on the fly)*, reliability *(buggy hosts can be removed through health monitoring which makes the system reliable)*, redundancy *(multiple paths leading to the same destination or failed server’s load is rerouted to the failover machine)*, and efficiency *(divide load evenly on all machines to use them effectively from the point of view of the service provider)* to the overall design of the system.

# What if load balancers fail? Are they not a single point of failure (SPOF)?

Load balancers are usually deployed in pairs as a means of disaster recovery. If one load balancer fails, and there’s nothing to failover to, the overall service will go down. Generally, to maintain high availability, enterprises use clusters of load balancers that use heartbeat communication to check the health of load balancers at all times. On failure of primary LB, the backup can take over. But, if the entire cluster fails, manual rerouting can also be performed in case of emergencies.
# [Algorithms](Algorithms.md)

# [[Global and Local Load Balancing]]

# [[Stateful versus stateless LBs]]

# OSI layers

![](../../../_Attachments/Pasted%20image%2020240104090917.png)

Load balancers (LB) are divided into two groups **Layer 4** or **Layer 7.** 

***[[LB4]]*** *can only act on data found in the transport and network layers like IP and TCP.* 
***[[LB7]]*** *can distribute requests based on data found in the application layer, like HTTP, which can allow you to do fancy things like siphon traffic to your new service. This mechanism has come in handy a few times when using the Strangler Pattern to migrate traffic from an old system to a new one.*

> Layer 7 load balancers are smart in terms of inspection. However layer 4 load balancers are faster in terms of processing.
# [[LB deployment]]

# [HAProxy](../Proxy/HAProxy.md)

# How should the response be routed?

The server can send the response directly to the routers (tier-1 LBs) through tier-3 LBs, which can forward the response from the data center. Such a response is called **direct routing** (**DR**) or **direct server return** (**DSR**). 

**Why don’t the servers directly send the response to the routers (tier-1 LBs) instead of tier-3 LBs?**

Tier-3 LBs maintain some state of connection—for example, SSL encryption/decryption. This is necessary to give clients a seamless experience.

# Load Balancers as a Service (LBaaS)

With the advent of the field of cloud computing, Load Balancers as a Service (LBaaS) has been introduced. This is where cloud owners provide load balancing services. Users pay according to their usage or the service-level agreement (SLA) with the cloud provider. Cloud-based LBs may not necessarily replace a local on-premise load balancing facility, but ***they can perform global traffic management between different zones***. Primary advantages of such load balancers include ease of use, management, metered cost, flexibility in terms of usage, auditing, and monitoring services to improve business decisions. An example of how cloud-based LBs can provide GSLB is given below:

![](../../../_Attachments/Pasted%20image%2020240119152757.png)

# References:

1. [Introduction to modern network load balancing and proxying](https://blog.envoyproxy.io/introduction-to-modern-network-load-balancing-and-proxying-a57f6ff80236)
2. [https://architecturenotes.co/load-balancers/](https://www.google.com/url?q=https://architecturenotes.co/load-balancers/&sa=D&source=calendar&usd=2&usg=AOvVaw3NIbOodYdko6gY8Ox2_whw) 
3. ! [Load Balancer — System Design Interview Question](https://medium.com/@anuupadhyay1994/load-balancer-system-design-interview-question-60e3ba231e3c)
4. [The Fundamental Knowledge of System Design — (9) — Load Balancer](https://medium.com/thedevproject/the-fundamental-knowledge-of-system-design-9-load-balancer-c55ff4feae5)
5. [What Is Load Balancing?](https://www.nginx.com/resources/glossary/load-balancing/)
6. [What is a Load Balancer? — Every Software Engineer must know](https://dineshchandgr.medium.com/what-is-a-load-balancer-every-software-engineer-must-know-21c0ea7ce6d4)
7. [What is Load Balancing?](https://www.youtube.com/watch?v=zaRkONvyGr8&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=5) (video)
8. [Load Balancer vs. Reverse Proxy vs. API Gateway](https://medium.com/geekculture/load-balancer-vs-reverse-proxy-vs-api-gateway-e9ec5809180c)
9. [Load Balancer vs Reverse Proxy (Explained by Example)](https://www.youtube.com/watch?v=S8J2fkN2FeI&list=PLQnljOFTspQUBSgBXilKhRMJ1ACqr7pTr&index=9) (video)
10. [Load balancer](https://github.com/donnemartin/system-design-primer#load-balancer) distribute incoming client requests to computing resources such as application servers and databases. In each case, the load balancer returns the response from the computing resource to the appropriate client.
11. [Why WebSockets over HTTP/2 (RFC8441) is Critical for Effective Load Balancing and Backend Scaling](https://www.youtube.com/watch?v=wLdxC9gesBs&list=PLQnljOFTspQUybacGRk1b_p13dgI-SmcZ&index=30) (video)
12. [Load Balancing in HTTP/2 Explained](https://www.youtube.com/watch?v=0avOYByiTRQ&list=PLQnljOFTspQWdgYcGXCTkjda8vd2jWJYt&index=6) (video)
13. [Why DNS Based Global Server Load Balancing (GSLB) Doesn’t Work](http://www.tenereillo.com/GSLBPageOfShame.htm)
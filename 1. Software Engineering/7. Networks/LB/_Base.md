A load balancer is a network device or software application that is used to evenly distribute incoming network traffic among multiple servers. Its primary purpose is to *==ensure optimal resource utilization, reduce latency, and maintain high availability of services==*. This is particularly important in scenarios where there are multiple servers that can handle incoming requests and where uneven distribution or sudden spikes in traffic can occur.

Load balancers employ various algorithms to determine how to distribute incoming traffic among the available servers.

***LB Benefits:***
1. Reduced downtime
2. Scalability
3. Redundancy
4. Flexibility
5. Efficiency

# [Algorithms](Algorithms.md)
## **Dynamic Load Balancing Algorithms**

### **1. Least Connection** 
Directs traffic to servers with the fewest active connections at the moment.
### **2. Weighted Least Connection** 
Administrators assign different weights to servers based on their capacity. Servers with higher weights can handle more connections and receive proportionally more traffic.
### **3. Weighted Response Time** 
Calculates the average response time of each server and combines it with the number of open connections to determine where to send traffic. It prioritizes servers with quicker response times for improved user experience.
### **4 Resource-based** 
Distributes load based on the resources available on each server, such as CPU and memory. Specialized software on servers measures resource availability, and the load balancer queries this information before directing traffic.

## Static Load Balancing Algorithms

### 1. Round Robin
Distributes traffic sequentially to each server in rotation. This can be implemented using DNS to return different IP addresses for each server, spreading the load.

### **2. Weighted Round Robin** 

Administrators assign weights to servers, indicating their relative capacity. Servers with higher weights receive more traffic, allowing for proportional load distribution.
### **3. IP Hash** 

The load balancer performs a hash function on the client’s IP address. This hash value determines which server receives the connection. This approach helps maintain session persistence by ensuring that requests from the same client always go to the same server.

# OSI layers

![](../../../_Attachments/Pasted%20image%2020240104090917.png)

Load balancers (LB) are divided into two groups **Layer 4** or **Layer 7.** 

***[[LB4]]*** *can only act on data found in the transport and network layers like IP and TCP.* 
***[[LB7]]*** *can distribute requests based on data found in the application layer, like HTTP, which can allow you to do fancy things like siphon traffic to your new service. This mechanism has come in handy a few times when using the Strangler Pattern to migrate traffic from an old system to a new one.*

# References:

1. [https://architecturenotes.co/load-balancers/](https://www.google.com/url?q=https://architecturenotes.co/load-balancers/&sa=D&source=calendar&usd=2&usg=AOvVaw3NIbOodYdko6gY8Ox2_whw) 
2. ! [Load Balancer — System Design Interview Question](https://medium.com/@anuupadhyay1994/load-balancer-system-design-interview-question-60e3ba231e3c)
3. [The Fundamental Knowledge of System Design — (9) — Load Balancer](https://medium.com/thedevproject/the-fundamental-knowledge-of-system-design-9-load-balancer-c55ff4feae5)
4. 4. [What Is Load Balancing?](https://www.nginx.com/resources/glossary/load-balancing/)
5. [What is a Load Balancer? — Every Software Engineer must know](https://dineshchandgr.medium.com/what-is-a-load-balancer-every-software-engineer-must-know-21c0ea7ce6d4)
6. [What is Load Balancing?](https://www.youtube.com/watch?v=zaRkONvyGr8&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=5) (video)
7. [Load Balancer vs. Reverse Proxy vs. API Gateway](https://medium.com/geekculture/load-balancer-vs-reverse-proxy-vs-api-gateway-e9ec5809180c)
8. [Load Balancer vs Reverse Proxy (Explained by Example)](https://www.youtube.com/watch?v=S8J2fkN2FeI&list=PLQnljOFTspQUBSgBXilKhRMJ1ACqr7pTr&index=9) (video)
9. [Load balancer](https://github.com/donnemartin/system-design-primer#load-balancer) distribute incoming client requests to computing resources such as application servers and databases. In each case, the load balancer returns the response from the computing resource to the appropriate client.
10. [Why WebSockets over HTTP/2 (RFC8441) is Critical for Effective Load Balancing and Backend Scaling](https://www.youtube.com/watch?v=wLdxC9gesBs&list=PLQnljOFTspQUybacGRk1b_p13dgI-SmcZ&index=30) (video)
11. [Load Balancing in HTTP/2 Explained](https://www.youtube.com/watch?v=0avOYByiTRQ&list=PLQnljOFTspQWdgYcGXCTkjda8vd2jWJYt&index=6) (video)
12. [Why DNS Based Global Server Load Balancing (GSLB) Doesn’t Work](http://www.tenereillo.com/GSLBPageOfShame.htm)
## **Dynamic Load Balancing Algorithms**

are algorithms that consider the current or recent state of the servers. Dynamic algorithms maintain state by communicating with the server, which **adds a communication overhead**. 
State maintenance makes the design of the algorithm much more complicated.

> In practice, dynamic algorithms provide far better results because they maintain a state of serving hosts and are, therefore, worth the effort and complexity.

### **1. Least Connection** 
Directs traffic to servers with the fewest active connections at the moment.

In certain cases, even if all the servers have the same capacity to serve clients, uneven load on certain servers is still a possibility. For example, some clients may have a request that requires longer to serve. Or some clients may have subsequent requests on the same connection. In that case, we can use algorithms like least connections where newer arriving requests are assigned to servers with fewer existing connections. LBs keep a state of the number and mapping of existing connections in such a scenario. We’ll discuss more about state maintenance later in the lesson.
### **2. Weighted Least Connection** 
Administrators assign different weights to servers based on their capacity. Servers with higher weights can handle more connections and receive proportionally more traffic.
### **3. Weighted Response Time** 
Calculates the average response time of each server and combines it with the number of open connections to determine where to send traffic. It prioritizes servers with quicker response times for improved user experience.
### **4 Resource-based** 
Distributes load based on the resources available on each server, such as CPU and memory. Specialized software on servers measures resource availability, and the load balancer queries this information before directing traffic.

## Static Load Balancing Algorithms

don’t consider the changing state of the servers. Therefore, task assignment is carried out based on existing knowledge about the server’s configuration. Naturally, these algorithms aren’t complex, and they get implemented in a single router or commodity machine where all the requests arrive.
### 1. Round Robin
Distributes traffic sequentially to each server in rotation. This can be implemented using DNS to return different IP addresses for each server, spreading the load.
	(-) uneven load distribution on end-servers
	(-) doesn’t consider any end-server crashes, it keeps on distributing the IP address of the crashed servers until the TTL of the cached entries expires. Availability of the service, in that case, can take a hit due to DNS-level load balancing

### **2. Weighted Round Robin** 
Administrators assign weights to servers, indicating their relative capacity. Servers with higher weights receive more traffic, allowing for proportional load distribution.
### **3. IP Hash** 
The load balancer performs a hash function on the client’s IP address. This hash value determines which server receives the connection. This approach helps maintain session persistence by ensuring that requests from the same client always go to the same server.
### 4. **URL hash**
It may be possible that some services within the application are provided by specific servers only. In that case, a client requesting service from a URL is assigned to a certain cluster or set of servers. The URL hashing algorithm is used in those scenarios.

# References:

1. [Сравнительный анализ методов балансировки трафика](https://habr.com/ru/companies/oleg-bunin/articles/319262/)
2. [Алгоритмы балансировки нагрузок](https://habr.com/ru/companies/ruvds/articles/732648/)
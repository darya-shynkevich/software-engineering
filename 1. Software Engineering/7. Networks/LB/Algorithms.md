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

# References:

1. [Сравнительный анализ методов балансировки трафика](https://habr.com/ru/companies/oleg-bunin/articles/319262/)
2. [Алгоритмы балансировки нагрузок](https://habr.com/ru/companies/ruvds/articles/732648/)
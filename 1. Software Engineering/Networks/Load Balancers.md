
Load balancers are responsible for ***how that load is distributed across the pool***:
1. ***Round Robin*** - Requests are distributed one at a time to each server.
2. ***IP Hashing*** - The IP address of the client is hashed and pinned to a specific server which handles the request. Useful in environments where session persistence or pinning to specific servers is essential.
3. ***Least load*** - Requests are distributed to the downstream server with the least concurrent connections.

***LB Benefits:***
1. Reduced downtime
2. Scalability
3. Redundancy
4. Flexibility
5. Efficiency


#### OSI layers
![Pasted image 20230605111431](../../_Attachments/Pasted%20image%2020230605111431.png)
Load balancers (LB) are divided into two groups **Layer 4** or **Layer 7.** 

***Layer 4 LBs*** *can only act on data found in the transport and network layers like IP and TCP.* 
***Layer 7 LBs*** *can distribute requests based on data found in the application layer, like HTTP, which can allow you to do fancy things like siphon traffic to your new service. This mechanism has come in handy a few times when using the Strangler Pattern to migrate traffic from an old system to a new one.*

###### Strangler Architecture Pattern

The Strangler Architecture Pattern conceals an old system by using a facade. The old system's services are gradually replaced by new external services behind the facade. The facade acts as a mediator between the old and new services, and over time, the old system's services are phased out in favour of the new ones.

#### Failover

Load balancers can help improve reliability and availability in distributed systems by monitoring server health and taking action if performance is poor. They can also quickly rotate out bad servers to avoid service gaps. Load balancers are essential tools for scaling architecture.


References:
1. [https://architecturenotes.co/load-balancers/](https://www.google.com/url?q=https://architecturenotes.co/load-balancers/&sa=D&source=calendar&usd=2&usg=AOvVaw3NIbOodYdko6gY8Ox2_whw) 
2. 
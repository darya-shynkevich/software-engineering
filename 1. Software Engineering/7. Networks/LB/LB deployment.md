In practice, however, a single layer LB isn’t enough for a large data center. In fact, multiple layers of load balancers coordinate to take informed forwarding decisions. A traditional data center may have a three-tier LB as shown below:

![](../../../_Attachments/Pasted%20image%2020240118231114.png)

## Tier-0 and tier-1 LBs

If DNS can be considered as the tier-0 load balancer, *equal cost multipath (ECMP) routers* are the tier-1 LBs. 

> Equal-cost multi-path routing (ECMP) is a routing strategy where packet forwarding to a single destination can occur over multiple best paths with equal routing priority. [Wikipedia]

From the name of ECMP, it’s evident that this layer divides incoming traffic on the basis of IP or some other algorithm like round-robin or weighted round-robin. Tier-1 LBs will balance the load across different paths to higher tiers of load balancers.

! ECMP routers play a vital role in the horizontal scalability of the higher-tier LBs.

## Tier-2 LBs

The second tier of LBs include layer 4 load balancers. Tier-2 LBs make sure that for any connection, all incoming packets are forwarded to the same tier-3 LBs. To achieve this goal, a technique like [[1. Software Engineering/2. Architecture/1. Base/1. Concepts/Consistent Hashing]] can be utilized. But in case of any changes to the infrastructure, consistent hashing may not be enough.

Tier-2 load balancers can be considered the glue between tier-1 and tier-3 LBs. ***Excluding tier-2 LBs could result in erroneous forwarding decisions in case of failures or dynamic scaling of LBs.***

## Tier-3 LBs

Layer 7 LBs provide services at tier 3. 
Since these LBs are in direct contact with the back-end servers, they perform health monitoring of servers at HTTP level. This tier enables scalability by evenly distributing requests among the set of healthy back-end servers and provides high availability by monitoring the health of servers directly. 
This tier also reduces the burden on end-servers by handling low-level details like TCP-congestion control protocols, the discovery of Path MTU (maximum transmission unit), the difference in application protocol between client and back-end servers, and so on. 
The idea is to leave the computation and data serving to the application servers and effectively utilize load balancing commodity machines for trivial tasks. In some cases, layer 7 LBs are at the same level as the service hosts.

# Summary

To summarize, tier 1 balances the load among the load balancers themselves. Tier 2 enables a smooth transition from tier 1 to tier 3 in case of failures, whereas tier 3 does the actual load balancing between back-end servers. Each tier performs other tasks to reduce the burden on end-servers.
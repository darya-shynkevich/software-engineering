1. **Global server load balancing (GSLB)**: GSLB involves the distribution of traffic load across multiple geographical regions.
2. **Local load balancing**: This refers to load balancing achieved within a data center. This type of load balancing focuses on improving efficiency and better resource utilisation of the hosting servers in a data center.

## Global server load balancing

GSLB ensures that globally arriving traffic load is intelligently forwarded to a data center. For example, power or network failure in a data center requires that all the traffic be rerouted to another data center. GSLB takes forwarding decisions based on the users’ geographic locations, the number of hosting servers in different locations, the health of data centers, and so on.

The illustration below shows that the GSLB can forward requests to three different data centers. Each local load balancing layer within a data center will maintain a control plane connection with the GSLB providing information about the health of the LBs and the server farm. GSLB uses this information to drive traffic decisions and forward traffic load based on each region’s configuration and monitoring information.

![](_Attachments/Pasted%20image%2020240118224951.png)


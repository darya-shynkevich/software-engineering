[Monitoring](Monitoring.md) helps in analysing such complex infrastructure where something is constantly failing. Monitoring distributed systems entails gathering, interpreting, and displaying data about the interactions between processes that are running at the same time. It assists in debugging, testing, performance evaluation, and having a bird’s-eye view over multiple services.

To avoid cascading failures, monitoring can play a vital role with early warnings or steering us to the root cause of faults.

! Having a monitoring system reduces operational costs and encourages an automated way to detect failures.

### What should I monitor?

1. Monitor critical local processes on a server for crashes.
2. Monitor any anomalies in the use of CPU/memory/disk/network bandwidth by a process on a server.
3. Monitor overall server health, such as CPU, memory, disk, network bandwidth, average load, and so on.
4. Monitor hardware component faults on a server, such as memory failures, failing or slowing disk, and so on.
5. Monitor the server’s ability to reach out-of-server critical services, such as network file systems and so on.
6. Monitor all network switches, load balancers, and any other specialized hardware inside a data center.
7. Monitor power consumption at the server, rack, and data center levels.
8. Monitor any power events on the servers, racks, and data center.
9. Monitor routing information and DNS for external clients.
10. Monitor network links and paths’ latency inside and across the data centers.
11. Monitor network status at the peering points.
12. Monitor overall service health that might span multiple data centers—for example, a CDN and its performance.

# References:

1. [**Distributed Monitoring and Alerting**](https://www.oreilly.com/ideas/monitoring-distributed-systems)
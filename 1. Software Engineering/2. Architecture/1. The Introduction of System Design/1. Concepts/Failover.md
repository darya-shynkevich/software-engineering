**Disadvantage(s):** Fail-over adds more hardware and additional complexity. There is a potential for loss of data if the active system fails before any newly written data can be replicated to the passive.

#### 1. **Active-passive**
With active-passive fail-over, heartbeats are sent between the active and the passive server on standby. If the heartbeat is interrupted, the passive server takes over the active's IP address and resumes service.

The length of downtime is determined by whether the passive server is already running in 'hot' standby or whether it needs to start up from 'cold' standby. Only the active server handles traffic.

Active-passive failover can also be referred to as master-slave failover.
#### 2. **Active-active**
In active-active, both servers are managing traffic, spreading the load between them.

If the servers are public-facing, the DNS would need to know about the public IPs of both servers. If the servers are internal-facing, application logic would need to know about both servers.

Active-active failover can also be referred to as master-master failover.
# References:

1. [**Failover**](http://cloudpatterns.org/mechanisms/failover_system)
2. [How to avoid cascading failures in a distributed system](https://www.youtube.com/watch?v=xrizarXJgC8&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=24) (video)
3. [Handling Failures in Distributed Systems!!](https://medium.com/@surfd1001/handling-failures-in-distributed-systems-ae6dab47da0c)
4. [The Evolution of Global Traffic Routing and Failover](https://www.usenix.org/conference/srecon16/program/presentation/heady)
5. [Testing for Disaster Recovery Failover Testing](https://www.usenix.org/conference/srecon17asia/program/presentation/liu_zehua)
6. [Designing a Microservices Architecture for Failure](https://blog.risingstack.com/designing-microservices-architecture-for-failure/)
7. [ELB for Automatic Failover at GoSquared](https://engineering.gosquared.com/use-elb-automatic-failover)
8. [Eliminate the Database for Higher Availability at American Express](http://americanexpress.io/eliminate-the-database-for-higher-availability/)
9. [Failover with Redis Sentinel at Vinted](http://engineering.vinted.com/2015/09/03/failover-with-redis-sentinel/)
10. [High-availability SaaS Infrastructure at FreeAgent](http://engineering.freeagent.com/2017/02/06/ha-infrastructure-without-breaking-the-bank/)
11. [MySQL High Availability at GitHub](https://github.blog/2018-06-20-mysql-high-availability-at-github/)
12. [MySQL High Availability at Eventbrite](https://www.eventbrite.com/engineering/mysql-high-availability-at-eventbrite/)
13. [Business Continuity & Disaster Recovery at Walmart](https://medium.com/walmartlabs/business-continuity-disaster-recovery-in-the-microservices-world-ef2adca363df)

[**Resilience Engineering: Learning to Embrace Failure**](https://queue.acm.org/detail.cfm?id=2371297)
	1. [Паттерны отказоустойчивой архитектуры / Александр Кривощёков (Яндекс Еда)](https://www.youtube.com/watch?v=WWTq-tbZwUE) (video)
	2. [Resilience Engineering with Project Waterbear at LinkedIn](https://engineering.linkedin.com/blog/2017/11/resilience-engineering-at-linkedin-with-project-waterbear)
	3. [Resiliency against Traffic Oversaturation at iHeartRadio](https://tech.iheart.com/resiliency-against-traffic-oversaturation-77c5ed92a5fb)
	4. [Resiliency in Distributed Systems at GO-JEK](https://blog.gojekengineering.com/resiliency-in-distributed-systems-efd30f74baf4)
	5. [Practical NoSQL Resilience Design Pattern for the Enterprise at eBay](https://www.ebayinc.com/stories/blogs/tech/practical-nosql-resilience-design-pattern-for-the-enterprise/)
	6. [Ensuring Resilience to Disaster at Quora](https://engineering.quora.com/Ensuring-Quoras-Resilience-to-Disaster)
	7. [Site Resiliency at Expedia](https://www.infoq.com/presentations/expedia-website-resiliency?utm_source=presentations_about_Case_Study&utm_medium=link&utm_campaign=Case_Study)
	8. [Resiliency and Disaster Recovery with Kafka at eBay](https://tech.ebayinc.com/engineering/resiliency-and-disaster-recovery-with-kafka/)
	9. [Disaster Recovery for Multi-Region Kafka at Uber](https://eng.uber.com/kafka/)
[**Availability in numbers](https://github.com/donnemartin/system-design-primer#availability-in-numbers)** Availability is often quantified by uptime (or downtime) as a percentage of time the service is available. Availability is generally measured in number of 9s--a service with 99.99% availability is described as having four 9s.

There are two complementary patterns to support high availability: **[Failover](../1.%20The%20Introduction%20of%20System%20Design/1.%20Concepts/Failover.md)** and **replication**.

# Important topics:

### 1. [Monitoring](../../Troubleshooting/Monitoring.md)

### 2. [Circuit Breaker](../1.%20The%20Introduction%20of%20System%20Design/1.%20Concepts/Circuit%20Breaker.md)

### 3.  Latency: [Base](../1.%20The%20Introduction%20of%20System%20Design/1.%20Concepts/Latency/Base.md)

### 4. [Failover](../1.%20The%20Introduction%20of%20System%20Design/1.%20Concepts/Failover.md)

### 5. [Load Balancing](../1.%20The%20Introduction%20of%20System%20Design/1.%20Concepts/Load%20Balancing.md)

### 6. [Autoscaling](../1.%20The%20Introduction%20of%20System%20Design/1.%20Concepts/Autoscaling.md)

# References:

1. **[A brief history of high availability](https://www.cockroachlabs.com/blog/brief-history-high-availability/?utm_source=substack&utm_medium=email)**
2. **[Resiliency in Distributed Systems](https://blog.pragmaticengineer.com/resiliency-in-distributed-systems/)**
3. [Availability and Single Points of Failure](https://docs.oracle.com/cd/E19693-01/819-0992/fjdch/index.html)
4. [Patterns for Resilient Architecture](https://medium.com/the-cloud-architect/patterns-for-resilient-architecture-part-3-16e8601c488e)
5. [Here's A 1300 Year Old Solution To Resilience - Rebuild, Rebuild, Rebuild](http://highscalability.com/blog/2014/4/23/heres-a-1300-year-old-solution-to-resilience-rebuild-rebuild.html)
6. [Data Access for Highly-Scalable Solutions: Using SQL, NoSQL, and Polyglot Persistence](https://docs.microsoft.com/en-us/previous-versions/msp-n-p/dn271399(v=pandp.10))
7. [The Calculus of Service Availability](https://queue.acm.org/detail.cfm?id=3096459&__s=dnkxuaws9pogqdnxmx8i)
8. [Breaking Things on Purpose](https://www.usenix.org/conference/srecon17americas/program/presentation/andrus)
9. [Building Fast and Resilient Web Applications - Ilya Grigorik](https://www.igvita.com/2016/05/20/building-fast-and-resilient-web-applications/)
10. [Accept Partial Failures, Minimize Service Loss](https://www.usenix.org/conference/srecon17asia/program/presentation/wang_daxin)
11. [Design for Resiliency](http://highscalability.com/blog/2012/12/31/designing-for-resiliency-will-be-so-2013.html)
12. [Design for Self-healing](https://docs.microsoft.com/en-us/azure/architecture/guide/design-principles/self-healing)
13. [NodeJS High Availability at Yahoo](https://yahooeng.tumblr.com/post/68823943185/nodejs-high-availability)
14. [Operations (11 parts) at LinkedIn](https://www.linkedin.com/pulse/introduction-every-day-monday-operations-benjamin-purgason)
15. [Monitoring Powers High Availability for LinkedIn Feed](https://www.usenix.org/conference/srecon17americas/program/presentation/barot)
16. [Supporting Global Events at Facebook](https://code.facebook.com/posts/166966743929963/how-production-engineers-support-global-events-on-facebook/)
17. [High Availability at BlaBlaCar](https://medium.com/blablacar-tech/the-expendables-backends-high-availability-at-blablacar-8cea3b95b26b)
18. [High Availability at Netflix](https://medium.com/@NetflixTechBlog/tips-for-high-availability-be0472f2599c)
19. [High Availability Cloud Infrastructure at Twilio](https://www.twilio.com/engineering/2011/12/12/scaling-high-availablity-infrastructure-in-cloud)
20. [Automating Datacenter Operations at Dropbox](https://blogs.dropbox.com/tech/2019/01/automating-datacenter-operations-at-dropbox/)
21. [Globalizing Player Accounts at Riot Games](https://technology.riotgames.com/news/globalizing-player-accounts)
22. [Throttling: Maintain a Steady Pace](http://www.sosp.org/2001/papers/welsh.pdf)
23. [Steady State: Always Put Logs on Separate Disk](https://docs.microsoft.com/en-us/sql/relational-databases/policy-based-management/place-data-and-log-files-on-separate-drives)
24. [Multi-Clustering: Improving Resiliency and Stability of a Large-scale Monolithic API Service at LinkedIn](https://engineering.linkedin.com/blog/2017/11/improving-resiliency-and-stability-of-a-large-scale-api)
25. [Our backend strategy to handle massive traffic](https://medium.com/coupang-engineering/our-backend-strategy-to-handle-massive-traffic-d30cd6cc4fb2)
26. [Designing Stripe for Black Friday: How to achieve 99.9999% uptime](https://learningdaily.dev/designing-stripe-for-black-friday-how-to-achieve-99-9999-uptime-cd720232c14)
27. [Reliability v/s Resiliency Design Strategies For Microservices](https://iamkanikamodi.medium.com/reliability-v-s-resiliency-design-strategies-for-microservices-8d15729da081)
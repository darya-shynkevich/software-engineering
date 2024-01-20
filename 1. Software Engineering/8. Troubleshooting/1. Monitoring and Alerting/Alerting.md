**Alerting** is the part of a monitoring system that responds to changes in metric values and takes action. There are two components to an alert definition: a metrics-based condition or threshold, and an action to take when the values fall outside of the permitted range.

## The levels of monitoring & alerts

![Pasted image 20231014191613](../../../_Attachments/Pasted%20image%2020231014191613.png)

### Operational alerts

The first type of alerts are the operation alerts. This category focusses on making sure that everything keeps running smoothly at the service/bare-metal level. These alerts monitor for example how many messages are in a certain queue and what the age of the oldest message is. This gives us a good idea if our processes are healthy.

### Data validation

The previous alert gave an overview of operational load & throughput. What it did not show was if certain values were received and if the amount of data was (more or less) correct. Other metrics that fall under data validation are:

- Did we receive (enough) files for a given date?
- Was the amount of data relatively close to the average amount of ingested rows of previous days?
- Can the incoming files be unzipped/loaded correctly?
- Did we receive the file at the correct timestamp?
- ...

The knowledge is quite superficial, as we're only looking at the **amount** of data and not the semantics of the data itself.

### Business Assumptions

Business assumptions come in all flavours. To give an idea, I'll give a few examples:

- We assume the header of the file always has a certain date format, but we're not sure if this will transfer to newly received files.
- We assume 2 values always add up to a third value. If this is incorrect, our next calculations cascade the faultiness.
- Are the values in a certain row correct according to our initial research?
- Are values within a certain range without too many outliers?
- ...

# References:

1. [**Distributed Monitoring and Alerting**](https://www.oreilly.com/ideas/monitoring-distributed-systems)
5. [Собственная система уведомлений о нештатных ситуациях](https://www.youtube.com/watch?v=U4u4Bd0FtrY&list=PLH-XmS0lSi_wMtn1TsBc2_vv7tBDAf7Qg&index=16) (video)*
21. [Securitybot: Distributed Alerting Bot at Dropbox](https://blogs.dropbox.com/tech/2017/02/meet-securitybot-open-sourcing-automated-security-at-scale/)
24. [Alerting Ecosystem at Uber](https://eng.uber.com/observability-at-scale/)
25. [Alerting Framework at Airbnb](https://medium.com/airbnb-engineering/alerting-framework-at-airbnb-35ba48df894f)
26. [Alerting on Service-Level Objectives (SLOs) at SoundCloud](https://developers.soundcloud.com/blog/alerting-on-slos
28. [Monitoring and Alert System using Graphite and Cabot at HackerEarth](http://engineering.hackerearth.com/2017/03/21/monitoring-and-alert-system-using-graphite-and-cabot/)
30. [Distributed Security Alerting at Slack](https://slack.engineering/distributed-security-alerting-c89414c992d6)
31. [Real-Time News Alerting at Bloomberg](https://www.infoq.com/presentations/news-alerting-bloomberg)
## [Distributed Monitoring](../1.%20Base/2.%20Components/Distributed%20Monitoring.md)

The system requirements are in "[What should I monitor?](../1.%20Base/2.%20Components/Distributed%20Monitoring.md)" section.

We want automated monitoring that identifies an anomaly in the system and informs the alert manager or shows the progress on a dashboard. Cloud service providers provide a health status of their services:
- AWS: [https://health.aws.amazon.com/health/status](https://health.aws.amazon.com/health/status)
- Azure: [https://status.azure.com/en-us/status](https://status.azure.com/en-us/status)
- Google: [https://status.cloud.google.com/](https://status.cloud.google.com/)

We’ll use blob storage to store our information about metrics.
## High-level design

The high-level components of our monitoring service are the following:
1. **Storage**: A time-series database stores metrics data, such as the current CPU use or the number of exceptions in an application.
2. **Data collector service**: This fetches the relevant data from each service and saves it in the storage.
3. **Querying service**: This is an API that can query on the time-series database and return the relevant information.

![](../../../_Attachments/Pasted%20image%2020240120145824.png)

### 1. Storage

We’ll use [time-series databases](../../3.%20Database/OLAP/Time%20Series%20Databases/Base.md) to save the data locally on the server where our monitoring service is running. Then, we’ll integrate it with a separate storage node. We’ll use blob storage to store our metrics.

We need to store metrics and know which action to perform if a metric has reached a particular value. For example, if CPU usage exceeds 90%, we generate an alert to the end user so the alert receiver can do take the necessary steps, such as allocate more resources to scale. ***For this purpose, we need another storage area that will contain the rules and actions.*** ***Let’s call it a rules database***. Upon any violation of the rules, we can take appropriate action.

Here, we have identified two more components in our design—that is, a rules and action database and a storage node (a blob store).

![](../../../_Attachments/Pasted%20image%2020240120150035.png)
### 2. Data collector

We need a monitoring system to update us about our several data centers. We can stay updated if the information about our processes reaches us, which is possible through logging. We’ll choose a pull strategy. Then, we’ll extract our relevant metrics from the logs of the application. As discussed in our logging design, we used a [distributed messaging queue](https://www.educative.io/collection/page/10370001/4941429335392256/4835612456124416#Using-distributed-messaging-queue). The message in the queue has the service name, ID, and a short description of the log. This will help us identify the metric and its information for a specific service. Exposing the relevant metrics to the data collector is necessary for monitoring any service so that our data collector can get the metrics from the service and store them into the time-series database.

! A real-world example of a monitoring system based on the pull-based approach is DigitalOcean. It monitors millions of machines that are dispersed globally.
#### 2.1 Service discoverer

The data collector is responsible for fetching metrics from the services it monitors. This way, the monitoring system doesn’t need to keep a track of services. Instead, it can find them using discoverer service. We’ll save the relative information of the services we have to monitor. We’ll use a service discovery solution and integrate with several platforms and tools, including EC2, Kubernetes, and Consul. This will allow us to discover which services we have to monitor. Similar dynamic discovery can be used for newly commissioned hardware.

![](../../../_Attachments/Pasted%20image%2020240120151012.png)

### 3. Querying service

We want a service to access the database and fetch the relevant query results. We need this because we want to view the errors like values of a particular node’s memory usage, or send an alert if a metric exceeds the set limit. Let’s add the two components we need along with querying.

#### 3.1 Alert Manager

The alert manager is responsible for sending alerts upon violations of set rules. It can send alerts as an email, a Slack message, and so on.

#### 3.2 Dashboard

We can set dashboards by using the collected metrics to display the required information—for example, the number of requests in the current week.

![](../../../_Attachments/Pasted%20image%2020240120151220.png)

### Pros and Cons of current Design

(+) the design of our monitoring service ensures the smooth working of the operations and keeps an eye on signs of impending problems.
(+) our design avoids overloading the network traffic by fetching the data itself.
(+) the monitoring service provides higher availability.

(-) the system seems scalable, but managing more servers to monitor can be a problem. For example, we have a dedicated server responsible for running the monitoring service. It can be a single point of failure (SPOF). To cater to SPOF, we can have a failover server for our monitoring system. Then, we also need to maintain consistency between actual and failover servers. However, such a design will also hit a scalability ceiling as the number of servers further increase.
(-) monitoring collects an enormous amount of data 24/7, and keeping it forever might not be feasible. We need a policy and mechanisms to delete unwanted data periodically to efficiently utilize the resources.

## Improving our design

In a push-based approach, the application pushes its data to the monitoring system.

![](../../../_Attachments/Pasted%20image%2020240120151456.png)

We’ll keep using a pull-based strategy for several servers within a data center. We’ll also assign several monitoring servers for hundreds or thousands of servers within a data center — let’s say one server monitoring 5,000 servers. We’ll call them secondary monitoring servers.

Now, we’ll apply the push-based strategy. The secondary monitoring systems will push their data to a primary data center server. The primary data center server will push its data to a global monitoring service responsible for checking all the data centers spread globally.

We’ll use blob storage to store our excessive data, apply elastic search, and view our relevant stats using a visualizer. As our servers or data centers increase, we’ll add more monitoring systems. The design for this is given below.

![](../../../_Attachments/Pasted%20image%2020240120151642.png)

![](../../../_Attachments/Pasted%20image%2020240120151653.png)

# Focus on Client-side Errors in a Monitoring System

- Failure in DNS name resolution.
- Any failure in routing along the path from the client to the service provider.
- Any failures with third-party infrastructure, such as middleboxes and content delivery networks (CDNs).

![](../../../_Attachments/Pasted%20image%2020240120152249.png)

- In a distributed system, it’s difficult to detect and respond to errors on the client side. So, it’s necessary to monitor such events to provide a good user experience.
- We can handle errors using an independent agent that sends service reports about any failures to a collector. Such collectors should be independent of the primary service in terms of infrastructure and deployment.

![](../../../_Attachments/Pasted%20image%2020240127150750.png)
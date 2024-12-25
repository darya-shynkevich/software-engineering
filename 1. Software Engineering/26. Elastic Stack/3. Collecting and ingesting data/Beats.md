Beats are lightweight applications (also referred to as agents) that can collect and ship several types of data to destinations such as [[Elasticsearch]], [[Logstash]], or [[Kafka]].

**When to use?**
1. When you need to collect data from a large number of hosts or systems from across your environment. 
   Examples:
	   1. ) Collecting web logs from a dynamic group of hundreds of web servers
	   2. Collecting logs from a large number of microservices running on a container orchestration platform such as Kubernetes
	   3. Collecting metrics from a group of MySQL instances in cloud/on-premises locations
2. When there is a supported Beats module available.
3. When you do not need to perform a significant amount of transformation/ processing before consuming data on Elasticsearch.
4. When consuming from a web source, you do not need to have scaling/throughput concerns in place for a single beat instance.

Elastic offers a few types of Beats today for various use cases: 
1. Filebeat: Collecting log data
2. Metricbeat: Collecting metric data
3. Packetbeat: Decoding and collecting network packet metadata
4. Heartbeat: Collecting system/service uptime and latency data
5. Auditbeat: Collecting OS audit data 
6. Winlogbeat: Collecting Windows event, applicatio, and security logs
7. Functionbeat: Running data collection on serverless compute infrastructure such as AWS Lambda

Beats use an open source library called `libbeat` that provides generic APIs for configuring inputs and destinations for data output. 
Beats implement the data collection functionality that's specific to the type of data (such as logs and metrics) that they collect. A range of community-developed Beats are available, in addition to the officially produced Beats agents.

### Onboarding and managing data sources at scale

Elastic Agent is a single, unified agent that can be used to collect logs, metrics, and uptime data from your environment. Elastic Agent orchestrates the core Beats agents under the hood but simplifies the deployment and configuration process, given teams now need to manage just one agent.
1. [Elasticsearch](Elasticsearch.md) is a full-text search engine and a versatile data store. It can store and allow you to search and compute aggregations on large volumes of data quickly
2. [[Kibana]] provides a user interface for Elasticsearch. Users can search for and create visualizations, and then administer Elasticsearch, using this tool. Kibana also offers out-of-the-box solutions (in the form of apps) for use cases such as search, security, and observability.
3. [[Beats]] can be used to collect and ship data directly from a range of source systems (such as different types of endpoints, network and infrastructure appliances, or cloud-based API sources) into [[Logstash]] or [[Elasticsearch]].
4. [[Logstash]] is an Extract, Transform, and Load ([[ETL]]) tool that's used to process and ingest data from various sources (such as log files on servers, Beats agents in your environment, or message queues and streaming platforms) into [[Elasticsearch]].

This diagram shows how the core components of the Elastic Stack work together to ingest, store, and search on data:

![](Pasted%20image%2020241225161131.png)




# References:

1. [Getting Started with Elastic Stack 8.0](https://www.amazon.com/Getting-Started-Elastic-Stack-8-0/dp/1800569491)
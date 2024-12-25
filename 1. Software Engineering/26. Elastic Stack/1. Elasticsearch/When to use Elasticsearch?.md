## 1. Ingesting, storing, and searching through large volumes of data

Elasticsearch is a **horizontally scalable data store** where **additional nodes can easily be added** to a cluster to increase the available resources.

Each node can store multiple primary shards on data, and each shard can be replicated (as replica shards) on other nodes. Primary shards handle read and write requests, while replica shards only handle read requests.

![](Pasted%20image%2020241225161921.png)
The preceding diagram shows the following: 
• Three Elasticsearch nodes: node A, node B, and node C 
• Two indices: index A and index B 
• Each index with two primary and two replica shards

Elasticsearch can also **store large volumes of data for search and aggregation**. 

> To retain data costs efficiently over long retention periods, clusters can be architected with **hot, warm, and cold tiers of data**. 
> *During its life cycle, data can be moved across nodes with different disk or Input/Output Operations Per Second (IOPS) specifications to take advantage of slower disk drives and their associated lower costs.*

Examples:
1. Centralized logging platforms (ingesting various application, security, event, and audit logs from multiple sources) 
2. When handling metrics/traces/telemetry data from many devices 
3. When ingesting data from large document repositories or crawling a large number of web pages

## 2. Getting relevant search results from textual data

As documents are ingested into [[Elasticsearch]], **unstructured textual components from the document are analyzed to extract some structure in the form of terms.**

**Terms are maintained in an inverted index data structure.**

![](Pasted%20image%2020241225162323.png)

A search string containing multiple terms goes through 1) a similar process of analysis to extract terms, to then 2) look up all the matching terms and their occurrences in the inverted index.

## 3. Aggregating data

1. **Bucket aggregations**: Bucket aggregations allow you to group (and sub-group) documents based on the values of fields or where the value sits in a range. 
2. **Metrics aggregations**: Metrics aggregations can calculate metrics based on the values of fields in documents. Supported metrics aggregations include avg, min, max, count, and cardinality, among others. Metrics can be computed for buckets/groups of data.

Tools such as Kibana heavily use aggregations to visualize the data on Elasticsearch.

## 4. Acting on data in near real time

Alerting would work by continually querying Elasticsearch (at a predefined interval) to return any documents that indicate degrading service performance or downtime. If the query returns any results, actions can be configured to alert a Site Reliability Engineer (SRE) or trigger automated remediation processes as appropriate.




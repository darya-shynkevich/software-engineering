*While [Beats](Beats.md) make it very convenient to onboard a new data source, they are designed to be lightweight in terms of performance footprint. As such, [Beats](Beats.md) do not provide a great deal of heavy processing, transformation, and enrichment capabilities. This is where Logstash comes in to help your ingestion architecture.*

Logstash is a general-purpose ETL tool designed to input data from any number of source systems/communication protocols. The data is then processed through a set of filters, where you can mutate, add, enrich, or remove fields as required. Finally, events can be sent to several destination systems.

**When to use?**
1. When a large amount of data is consumed from a centralized location (such as a file share, AWS S3, Kafka, and AWS Kinesis) and you need to be able to scale ingestion throughput.
2. When you need to transform data considerably or parse complex schemas/codecs, especially using regular expressions or Grok.
3. When you need to be able to load balance ingestion across multiple Logstash instances.
4. When a supported Beats module is not available.


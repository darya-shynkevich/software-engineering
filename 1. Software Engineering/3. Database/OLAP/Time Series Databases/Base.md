A **time series database (TSDB)** is designed to efficiently store, retrieve, and manage data that is organised and indexed by time. It traditionally works like a ***columnar key-value database*** but has time stamps as keys. If the timestamp alone can’t uniquely identify rows, we can combine it with another key (or keys) to form a composite key.

Examples:
1. Amazon Timestream
2. CrateDB
3. InfluxDB
4. [[TimescaleDB]]
5. Prometheus

(+) **time-based indexing:** they’re optimized for time-based indexing and querying, which is crucial for time series data;
(+) **aggregation:** TSDBs provide built-in functions for summarizing data over time intervals;
(+) **cost efficiency:** they can be more cost-effective for storing and processing time series data;
(+) **scalability:** Many TSDBs scale horizontally with relative simplicity to handle consumption needs.

However, they’re _inefficient_ for general data storage needs because they’re a niche use-case-specific type of database. So, it makes sense to use TSDBs only in combination with other databases in time-sensitive applications.
## Time series data

Time series data usually includes timestamped values such as stock prices, sensor readings, temperature measurements, network traffic data, etc. It’s usually represented by a graph, as shown below.

![](../../../../_Attachments/Pasted%20image%2020240120144853.png)
This kind of data can be stored in a TSDB, as shown in the table below. **Row 1** has the “2023-08-27 12:10:16” timestamp, which is the identifier or key for this row. Against this key, we have the crypto symbol “ETH” and a “+3.5” gain.

![](../../../../_Attachments/Pasted%20image%2020240120144915.png)

We can turn regular relational databases into TSDBs using add-ons, but since recently, standalone solutions in the market can provide a plug-and-play setup experience.
# References:

1. [Time series database (TSDB) explained](https://www.influxdata.com/time-series-database/#download)
2. [Time series данные в реляционной СУБД](https://www.youtube.com/watch?v=3WkNp7mllv0&list=PLH-XmS0lSi_yY4rQCIZyx5Np57zc77OyE&index=8) (video)
3. [Pintrest Time Series Database](https://medium.com/pinterest-engineering/goku-building-a-scalable-and-high-performant-time-series-database-system-a8ff5758a181)
4. [Uber Time Series DB](https://eng.uber.com/aresdb/)
5. [TimeSeries Relational DB](https://blog.timescale.com/blog/time-series-data-why-and-how-to-use-a-relational-database-instead-of-nosql-d0cd6975e87c/)
6. [Facebook Gorilla Time Series DB](http://www.vldb.org/pvldb/vol8/p1816-teller.pdf)
7. [Beringei: High-performance Time Series Storage Engine at Facebook](https://code.facebook.com/posts/952820474848503/beringei-a-high-performance-time-series-storage-engine/)
8. [MetricsDB: TimeSeries Database for storing metrics at Twitter](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2019/metricsdb.html)
9. [Atlas: In-memory Dimensional Time Series Database at Netflix](https://medium.com/netflix-techblog/introducing-atlas-netflixs-primary-telemetry-platform-bd31f4d8ed9a)
10. [Heroic: Time Series Database at Spotify](https://labs.spotify.com/2015/11/17/monitoring-at-spotify-introducing-heroic/)
11. [Roshi: Distributed Storage System for Time-Series Event at SoundCloud](https://developers.soundcloud.com/blog/roshi-a-crdt-system-for-timestamped-events)
12. [Goku: Time Series Database at Pinterest](https://medium.com/@Pinterest_Engineering/goku-building-a-scalable-and-high-performant-time-series-database-system-a8ff5758a181)
13. [Scaling Time Series Data Storage (2 parts) at Netflix](https://medium.com/netflix-techblog/scaling-time-series-data-storage-part-ii-d67939655586)
14. [Druid - Real-time Analytics Database](https://druid.apache.org/)
    1. [Druid at Airbnb](https://medium.com/airbnb-engineering/druid-airbnb-data-platform-601c312f2a4c)
    2. [Druid at Walmart](https://medium.com/walmartlabs/event-stream-analytics-at-walmart-with-druid-dcf1a37ceda7)
    3. [Druid at eBay](https://tech.ebayinc.com/engineering/monitoring-at-ebay-with-druid/)
    4. [Druid at Netflix](https://netflixtechblog.com/how-netflix-uses-druid-for-real-time-insights-to-ensure-a-high-quality-experience-19e1e8568d06)
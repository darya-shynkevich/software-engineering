[autometrics-py](https://github.com/autometrics-dev/autometrics-py) provides a decorator that makes it trivial to instrument any function with the most useful metrics: request rate, error rate, and latency. It standardizes these metrics and then generates powerful Prometheus queries based on your function details to help you quickly identify and debug issues in production.

[Percentiles](8.%20Troubleshooting/1.%20Monitoring%20and%20Alerting/Percentiles.md)

! We need to collect values of metrics with minimal performance penalty.

Values that track how many physical resources our operating system uses can be a good starting point. If we have a monitoring system in place, we don’t have to do much additional work to get data regarding **processor load**, **CPU statistics** like **cache hits and misses**,** RAM usage** by OS and processes, **page faults**, **disc space**, **disc read** and **write latencies**, **swap space usage**, and so on. Metrics provided by many web servers, database servers, and other software help us determine whether everything is running smoothly or not.
### Populate the metrics

1. in the pull model, monitoring system asks distributed data collectors to give data that they collected locally. This means that the data flow happens only when requested by the monitoring system; 
		(-) there can be situations when firewall prevents the monitoring system from accessing the server directly
2. in the push model, distributed collectors send their collected data to the monitoring system periodically (OpenTelemetry)
		(-) servers sending too much data or sending data too frequently can’t overload the monitoring system => can overwhelm the infrastructure with continual push requests from all services, resulting in network floods. 
		(+) each service can decide when to send metrics

### Persist the data

Figuring out how to store the metrics from the servers that are being monitored is important. A centralized in-memory metrics repository may be all that’s needed. However, for a large data center with millions of things to monitor, there will be an enormous amount of data to store, and a [time-series database](../../3.%20Database/OLAP/Time%20Series%20Databases/Base.md) can help in this regard.

[Time-series database](../../3.%20Database/OLAP/Time%20Series%20Databases/Base.md) help maintain durability, which is an important factor. Without a historical view of events in a monitoring system, it isn’t very useful. Samples having a value of time stamp are stored in chronological sequence. So, a whole metric’s timeline can be shown in the form of a time series.
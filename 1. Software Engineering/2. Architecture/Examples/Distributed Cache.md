A [Cache](../1.%20Base/2.%20Components/Cache/Cache.md) is a temporary data storage that can serve data faster by keeping data entries in memory. Caches store only the most frequently accessed data. When a request reaches the serving host, it retrieves data from the cache (**cache hit**) and serves the user. However, if the data is unavailable in the cache (**cache miss**), the data will be queried from the database. Also, the cache is populated with the new value to avoid cache misses for the next time.



# References:

1. [System Design — Distributed Cache](https://medium.com/@udayansawant7/system-design-distributed-cache-c7234b6c5e3e)
2. [Ubiquitous Caching: a Journey of Building Efficient Distributed and In-Process Caches at Twitter](https://www.infoq.com/presentations/trends-caches/)
3. [Design a key-value cache to save the results of the most recent web server queries](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/query_cache/README.md) (solution)
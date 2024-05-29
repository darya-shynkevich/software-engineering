A [Cache](0.%20Cache.md) is a temporary data storage that can serve data faster by keeping data entries in memory. Caches store only the most frequently accessed data. When a request reaches the serving host, it retrieves data from the cache (**cache hit**) and serves the user. However, if the data is unavailable in the cache (**cache miss**), the data will be queried from the database. Also, the cache is populated with the new value to avoid cache misses for the next time.
# [Writing policies](Writing%20policies.md)

# [Eviction policies](Eviction%20policies.md)

# [Cache Invalidation](Cache%20Invalidation.md)

# Storage mechanism

## Hash function

It’s possible to use hashing in two different scenarios:
- Identify the cache server in a distributed cache to store and retrieve data.
- Locate cache entries inside each cache server.

For the first scenario, we can use different hashing algorithms. However, [Consistent Hashing](Consistent%20Hashing.md) or its flavours usually perform well in distributed systems because simple hashing won’t be ideal in case of crashes or scaling.

In the second scenario, we can use typical hash functions to locate a cache entry to read or write inside a cache server. However, a hash function alone can only locate a cache entry. It doesn’t say anything about managing data within the cache server. That is, it doesn’t say anything about how to implement a strategy to evict less frequently accessed data from the cache server. It also doesn’t say anything about what data structures are used to store the data within the cache servers. This is exactly the second design question of the storage mechanism. Let’s take a look at the data structure next.
## Linked list + [SkipList](SkipList.md)

We’ll use a doubly linked list. The main reason is its widespread usage and simplicity. Furthermore, adding and removing data from the doubly linked list in our case will be a constant time operation. This is because we either evict a specific entry from the tail of the linked list or relocate an entry to the head of the doubly linked list. Therefore, no iterations are required.
## [Bloom Filter](Bloom%20Filter.md)

Can be used to check if `key` is in cache or not.
## Sharding in cache clusters

#### Dedicated cache servers

- There’s flexibility in terms of hardware choices for each functionality.
- It’s possible to scale web/application servers and cache servers separately.
#### Co-located cache

The main advantage of this strategy is the reduction in CAPEX and OPEX of extra hardware. Furthermore, with the scaling of one service, automatic scaling of the other service is obtained. However, the failure of one machine will result in the loss of both services simultaneously
# Cache client

- Each cache client will know about all the cache servers.
- All clients can use well-known transport protocols like TCP or UDP to talk to the cache servers.

# High-level Design of a Distributed Cache
## Requirements

Let us start by understanding the requirements of our solution.
### Functional
The following are the functional requirements:
- **Insert data:** The user of a distributed cache system must be able to insert an entry to the cache.
- **Retrieve data:** The user should be able to retrieve data corresponding to a specific key.
### Non-functional requirements
We’ll consider the following non-functional requirements:

- **High performance**: The primary reason for the cache is to enable fast retrieval of data. Therefore, both the `insert` and `retrieve` operations must be fast.
- **Scalability:** The cache system should scale horizontally with no bottlenecks on an increasing number of requests.
- **High availability**: The unavailability of the cache will put an extra burden on the database servers, which can also go down at peak load intervals. We also require our system to survive occasional failures of components and network, as well as power outages.
- **Consistency:** Data stored on the cache servers should be consistent. For example, different cache clients retrieving the same data from different cache servers (primary or secondary) should be up to date.
- **Affordability:** Ideally, the caching system should be designed from commodity hardware instead of an expensive supporting component within the design of a system.

## API design

```whiteText
insert(key, value)
```

```whiteText
retrieve(key)
```

# High-level design

![](Pasted%20image%2020240127191408.png)

- **Cache client**: This library resides in the service application servers. It holds all the information regarding cache servers. The cache client will choose one of the cache servers using a hash and search algorithm for each incoming `insert` and `retrieve` request. All the cache clients should have a consistent view of all the cache servers. Also, the resolution technique to move data to and from the cache servers should be the same. Otherwise, different clients will request different servers for the same data.
- **Cache servers**: These servers maintain the cache of the data. Each cache server is accessible by all the cache clients. Each server is connected to the database to store or retrieve data. Cache clients use TCP or UDP protocol to perform data transfer to or from the cache servers. However, if any cache server is down, requests to those servers are resolved as a missed cache by the cache clients.

![](Pasted%20image%2020240127191709.png)
- The client’s requests reach the service hosts through the load balancers where the cache clients reside.
- Each cache client uses consistent hashing to identify the cache server. Next, the cache client forwards the request to the cache server maintaining a specific shard.
- Each cache server has primary and replica servers. Internally, every server uses the same mechanisms to store and evict cache entries.
- Configuration service ensures that all the clients see an updated and consistent view of the cache servers.
- Monitoring services can be additionally used to log and report different metrics of the caching service.

### Maintain cache servers list

- **Solution 1**: It’s possible to have a configuration file in each of the service hosts where the cache clients reside. The configuration file will contain the updated health and metadata required for the cache clients to utilize the cache servers efficiently. Each copy of the configuration file can be updated through a push service by any DevOps tool. The main problem with this strategy is that the configuration file will have to be manually updated and deployed through some DevOps tools.
- **Solution 2**: We can store the configuration file in a centralized location that the cache clients can use to get updated information about cache servers. This solves the deployment issue, but we still need to manually update the configuration file and monitor the health of each server.
- **Solution 3**: An automatic way of handling the issue is to use a configuration service that continuously monitors the health of the cache servers. In addition to that, the cache clients will get notified when a new cache server is added to the cluster. When we use this strategy, no human intervention or monitoring will be required in case of failures or the addition of new nodes. Finally, the cache clients obtain the list of cache servers from the configuration service.

*The configuration service has the highest operational cost. At the same time, it’s a complex solution. However, it’s the most robust among all the solutions we presented.*
### Improve availability

 We can start by adding one primary and two backup nodes in a cache shard. With replicas, there’s always a possibility of inconsistency. If our replicas are in close proximity, writing over replicas is performed synchronously to avoid inconsistencies between shard replicas. It’s crucial to divide cache data among shards so that neither the problem of unavailability arises nor any hardware is wasted.

This solution has two main advantages:
- There’s improved availability in case of failures.
- Hot shards can have multiple nodes (primary-secondary) for reads.

### Internals of cache server

Each cache client should use three mechanisms to store and evict entries from the cache servers

- **Hash map**: The cache server uses a hash map to store or locate different entries inside the RAM of cache servers. The illustration below shows that the map contains pointers to each cache value.
- **Doubly linked list**: If we have to evict data from the cache, we require a linked list so that we can order entries according to their frequency of access. The illustration below depicts how entries are connected using a doubly linked list.
- **Eviction policy**: The eviction policy depends on the application requirements. Here, we assume the least recently used (LRU) eviction policy.

![](Pasted%20image%2020240127192000.png)


# References:

1. [System Design — Distributed Cache](https://medium.com/@udayansawant7/system-design-distributed-cache-c7234b6c5e3e)
2. [Ubiquitous Caching: a Journey of Building Efficient Distributed and In-Process Caches at Twitter](https://www.infoq.com/presentations/trends-caches/)
3. [Design a key-value cache to save the results of the most recent web server queries](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/query_cache/README.md) (solution)
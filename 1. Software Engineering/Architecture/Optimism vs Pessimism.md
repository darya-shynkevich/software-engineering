If there is no coordination in the system => components need make assumptions about what other components are doing. If two components can’t check in with each other after every single step, they need to make assumptions about the ongoing behavior of the other component.

Optimistic assumptions = avoid or delay coordination (assumes it’ll get away with its plans)
Pessimistic assumptions = require or seek coordination (takes the bull by the horns and makes sure it will)
# [[Cache]]

[[Distributed caching]] typically isn’t _coherent_, but we still want it to be _eventually consistent_. By _eventually consistent_ we mean that if the write stream stops, the caches eventually all converge on containing the same data. In other words, ***inconsistencies are relatively short-lived.***

Possibly the most common way of ensuring this property — that inconsistencies are short-lived—is with a time to live (TTL). This simply means that the cache only keeps items around for a certain fixed period of time. It’s also a _pessimistic_ one: ***the cache is doing extra work assuming that the item has changed.*** In systems with a low per-item write rate, that pessimistic assumption can be wrong much more often than it’s right.

The pessimistic TTL approach has ***a strong availability disadvantage***: if a network partition or authority downtime lasts longer than the TTL, the cache hit rate will drop to zero => 

**Solutions**
1. Try fetch the new item, but then _optimistically_ continue to use the old one if that’s possible (optimistic because it’s making the optimistic assumption that the item hasn’t change).
2. Asynchronously try fetch the new item, and use the old one until that can complete.

These protocol seem very similar to TTL, but are deeply fundamentally different. ***They don’t offer strong recency or staleness guarantees, but can tolerate indefinite network partitions***

# [[Concurrency control]]

# [[Leases]]


Often, cache stores a copy (or part) of data, which is persistently stored in a data store. When we store data to the data store, some important questions arise:
1. Where do we store the data first? Database or cache?
2. What will be the implication of each strategy for consistency models?
# [Cache Invalidation](Cache%20Invalidation.md)

# Writing Strategies

## Write-through cache

1. WRITE: write `key=value` to DB and cache
2. READ: read `key` from cache

Under this scheme, ***data is written into the cache and the corresponding database simultaneously***. Writing on both storages can happen concurrently or one after the other. 

(-) increases the write latency 
(-) requires a lot of space
(+) fast retrieval
(+) ensures strong consistency between the database and the cache
(+) nothing will get lost in case of a crash, power failure, or other system disruptions

**Example:** An e-commerce website updates its product inventory in real-time. Whenever a product's stock changes, the cache is also updated to reflect the new inventory count.

However the data loss can occur: [Cache Invalidation](Cache%20Invalidation.md)

![Pasted image 20231014201619](../../../../../_Attachments/Pasted%20image%2020231014201619.png)

## Write-around cache

1. WRITE: write `key=value` to DB
2. READ: read `key` from cache => 
	1. IF no `key` in cache => read `value` from DB and add it to cache
	2. IF in cache => return `value`

This strategy involves ***writing data to the database only***. Later, when a read is triggered for the ***data***, it’s ***written to cache after a cache miss***. The database will have updated data, but such a strategy isn’t favourable for reading recently updated data.

(-) read request for recently written data will create a ***==“cache miss”==*** and must be read from slower back-end storage and experience higher latency
(+) reduce the cache being flooded with write operations that will not subsequently be re-read (in comparison to write-through cache)

**Example:** An application updates user profile information, which is infrequently accessed. The application writes the new data directly to the data store, avoiding unnecessary cache updates.

However the data loss can occur: [Cache Invalidation](Cache%20Invalidation.md)

![Pasted image 20231014201715](../../../../../_Attachments/Pasted%20image%2020231014201715.png)

## Write-back cache (or lazy-write)

1. WRITE: write `key=value` to cache
2. WRITE: write `key=value` asynchronously to DB based on certain condition
3. READ: read `key` from cache => 
	1. IF no `key` in cache => read `value` from DB and add it to cache
	2. IF in cache => return `value`

In the write-back cache mechanism, the data is first written to the cache and completion is immediately confirmed to the client, and then data is asynchronously written to the database. Although the cache has updated data, inconsistency is inevitable in scenarios where a client reads stale data from the database. However, systems using this strategy will have small writing latency.

(-) inconsistency
(-) risk of data loss in case of a crash or other adverse event because the only copy of the written data is in the cache
(+) low-latency and high-throughput for write-intensive applications

***The write to the permanent storage is done*** based on certain conditions, for example, ***when the system needs some free space***. 

**Example:** Imagine a collaborative document editing application that allows multiple users to make changes to a document simultaneously. When users make changes, those changes are first saved to the cache, allowing the application to respond quickly and provide a smooth editing experience. When certain conditions are met (e.g., the number of changes reaches a certain threshold), the application writes the cached changes back to the data store, updating the document with the latest changes from all users. This approach minimizes the number of write operations to the data store and reduces the load on the storage system, improving the overall performance of the application. ([[Google Docs]])
## Write-behind cache

It is quite similar to write-back cache. In this scheme, data is written to the cache and acknowledged to the application immediately, but it is not immediately written to the permanent storage. Instead, the write operation is deferred, and the data is eventually written to the permanent storage at a later time. ***The main difference between write-back cache and write-behind cache is ==when== the data is written to the permanent storage.*** In write-back caching, data is only written to the permanent storage when it is necessary for the cache to free up space or when an event happens, while in ***==write-behind caching, data is written to the permanent storage at specified intervals==***.

**Example:** A document editor application temporarily saves changes to the cache while the user is editing. Periodically, the changes are written back to the data store to minimize the number of write operations. ([[Google Docs]])

![Pasted image 20231014172227](../../../../../_Attachments/Pasted%20image%2020231014172227.png)


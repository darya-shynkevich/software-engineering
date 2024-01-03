*==Cache invalidation is the process of removing or updating outdated data from a cache to ensure that only the most recent and accurate information is stored.==*

# Cache Invalidation Strategies

## Write-through cache

Under this scheme, ***data is written into the cache and the corresponding database simultaneously***. The cached data allows for fast retrieval and, since the same data gets written in the permanent storage, we will have ***==complete data consistency== between the cache and the storage***. Also, this scheme ensures that nothing will get lost in case of a crash, power failure, or other system disruptions. Although, write-through ==***minimizes the risk of data loss***==, since every write operation must be done twice before returning success to the client, this scheme has the disadvantage of ***==higher latency for write operations**==* ***==+ a lot of space==***

**Example:** An e-commerce website updates its product inventory in real-time. Whenever a product's stock changes, the cache is also updated to reflect the new inventory count.

However the data loss can occur: [Cache Invalidation - Data loss solution](Cache%20Invalidation%20-%20Data%20loss%20solution.md)

![Pasted image 20231014201619](../../../../../_Attachments/Pasted%20image%2020231014201619.png)

## Write-around cache

This technique is similar to write-through cache, but ***data is written directly to permanent storage, bypassing the cache***. This can ***==reduce the cache being flooded==*** with write operations that will not subsequently be re-read, but has the disadvantage that a read request for recently written data will create a ***==“cache miss”==*** and must be read from slower back-end storage and experience higher latency.

Update cache in the background???

**Example:** An application updates user profile information, which is infrequently accessed. The application writes the new data directly to the data store, avoiding unnecessary cache updates.

However the data loss can occur: [Cache Invalidation - Data loss solution](Cache%20Invalidation%20-%20Data%20loss%20solution.md)

![Pasted image 20231014201715](../../../../../_Attachments/Pasted%20image%2020231014201715.png)

## Write-back cache (or lazy-write)

Under this scheme, ***data is written to cache alone***, and completion is immediately confirmed to the client. ***The write to the permanent storage is done*** based on certain conditions, for example, ***when the system needs some free space***. This results in *==low-latency and high-throughput for write-intensive applications; however, this speed comes with the risk of data loss in case of a crash or other adverse event because the only copy of the written data is in the cache.==*

**Example:** Imagine a collaborative document editing application that allows multiple users to make changes to a document simultaneously. When users make changes, those changes are first saved to the cache, allowing the application to respond quickly and provide a smooth editing experience. When certain conditions are met (e.g., the number of changes reaches a certain threshold), the application writes the cached changes back to the data store, updating the document with the latest changes from all users. This approach minimizes the number of write operations to the data store and reduces the load on the storage system, improving the overall performance of the application. (Google Documents)

## Write-behind cache

It is quite similar to write-back cache. In this scheme, data is written to the cache and acknowledged to the application immediately, but it is not immediately written to the permanent storage. Instead, the write operation is deferred, and the data is eventually written to the permanent storage at a later time. ***The main difference between write-back cache and write-behind cache is ==when== the data is written to the permanent storage.*** In write-back caching, data is only written to the permanent storage when it is necessary for the cache to free up space or when an event happens, while in ***==write-behind caching, data is written to the permanent storage at specified intervals==***.

**Example:** A document editor application temporarily saves changes to the cache while the user is editing. Periodically, the changes are written back to the data store to minimize the number of write operations. (Google Documents)

![Pasted image 20231014172227](../../../../../_Attachments/Pasted%20image%2020231014172227.png)





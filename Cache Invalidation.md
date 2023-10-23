*==Cache invalidation is the process of removing or updating outdated data from a cache to ensure that only the most recent and accurate information is stored.==*

# Cache Invalidation Strategies

## Write-through cache

Under this scheme, ***data is written into the cache and the corresponding database simultaneously***. The cached data allows for fast retrieval and, since the same data gets written in the permanent storage, we will have ***==complete data consistency== between the cache and the storage***. Also, this scheme ensures that nothing will get lost in case of a crash, power failure, or other system disruptions. Although, write-through ==***minimizes the risk of data loss***==, since every write operation must be done twice before returning success to the client, this scheme has the disadvantage of ***==higher latency for write operations**==* ***==+ a lot of space==***

**Example:** An e-commerce website updates its product inventory in real-time. Whenever a product's stock changes, the cache is also updated to reflect the new inventory count.

However the data loss can occur: [[Cache Invalidation - Data loss solution]]

![[Pasted image 20231014201619.png]]

## Write-around cache

This technique is similar to write-through cache, but ***data is written directly to permanent storage, bypassing the cache***. This can ***==reduce the cache being flooded==*** with write operations that will not subsequently be re-read, but has the disadvantage that a read request for recently written data will create a ***==“cache miss”==*** and must be read from slower back-end storage and experience higher latency.

Update cache in the background???

**Example:** An application updates user profile information, which is infrequently accessed. The application writes the new data directly to the data store, avoiding unnecessary cache updates.

However the data loss can occur: [[Cache Invalidation - Data loss solution]]

![[Pasted image 20231014201715.png]]

## Write-back cache (or lazy-write)

Under this scheme, ***data is written to cache alone***, and completion is immediately confirmed to the client. ***The write to the permanent storage is done*** based on certain conditions, for example, ***when the system needs some free space***. This results in *==low-latency and high-throughput for write-intensive applications; however, this speed comes with the risk of data loss in case of a crash or other adverse event because the only copy of the written data is in the cache.==*

**Example:** Imagine a collaborative document editing application that allows multiple users to make changes to a document simultaneously. When users make changes, those changes are first saved to the cache, allowing the application to respond quickly and provide a smooth editing experience. When certain conditions are met (e.g., the number of changes reaches a certain threshold), the application writes the cached changes back to the data store, updating the document with the latest changes from all users. This approach minimizes the number of write operations to the data store and reduces the load on the storage system, improving the overall performance of the application. (Google Documents)

## Write-behind cache

It is quite similar to write-back cache. In this scheme, data is written to the cache and acknowledged to the application immediately, but it is not immediately written to the permanent storage. Instead, the write operation is deferred, and the data is eventually written to the permanent storage at a later time. ***The main difference between write-back cache and write-behind cache is ==when== the data is written to the permanent storage.*** In write-back caching, data is only written to the permanent storage when it is necessary for the cache to free up space or when an event happens, while in ***==write-behind caching, data is written to the permanent storage at specified intervals==***.

**Example:** A document editor application temporarily saves changes to the cache while the user is editing. Periodically, the changes are written back to the data store to minimize the number of write operations. (Google Documents)

![[Pasted image 20231014172227.png]]


# Cache Invalidations Methods

![[Pasted image 20231014172348.png]]

## Purge

The purge method removes cached content for a specific object, URL, or a set of URLs. It’s typically used when there is an update or change to the content and the cached version is no longer valid. When a purge request is received, the cached content is immediately removed, and the next request for the content will be served directly from the origin server.

**Example:** A news website purges a specific article from its cache after significant updates have been made, ensuring that users receive the latest version.

## Refresh

The refresh method retrieves requested content from the origin server, even if a cached version is available. When a refresh request is received, the cache updates the content with the latest version from the origin server, ensuring up-to-date information. Unlike a purge, a refresh request does not remove the existing cached content but updates it with the most recent version.

**Example:** An e-commerce website refreshes the cache of a product page when a new sale is launched to display the updated pricing information.

## Ban

The ban method invalidates cached content based on specific criteria, such as a URL pattern or header. Upon receiving a ban request, any cached content matching the specified criteria is immediately removed. Subsequent requests for the content will be served directly from the origin server, ensuring that users receive the most recent and relevant information.

**Example:** A content management system bans all cached content with a specific tag when that tag is modified, ensuring that users only see the updated content.

## Time-to-live (TTL) expiration

This method involves setting a time-to-live value for cached content, after which the content is considered stale and must be refreshed. When a request is received for the content, the cache checks the time-to-live value and serves the cached content only if the value hasn’t expired. If the value has expired, the cache fetches the latest version of the content from the origin server and caches it.

**Example:** A weather website sets a 1-hour TTL for its weather forecast data, ensuring that users receive relatively up-to-date weather information without overloading the origin server.

## Stale-while-revalidate

This method is used in web browsers and CDNs to serve stale content from the cache while the content is being updated in the background. When a request is received for a piece of content, the cached version is immediately served to the user, and an asynchronous request is made to the origin server to fetch the latest version of the content. Once the latest version is available, the cached version is updated. This method ensures that the ***user is always served content quickly, even if the cached version is slightly outdated***.

**Example:** A media streaming platform uses stale-while-revalidate to serve video thumbnails, ensuring that users can quickly browse the catalog while the platform updates thumbnail images in the background.


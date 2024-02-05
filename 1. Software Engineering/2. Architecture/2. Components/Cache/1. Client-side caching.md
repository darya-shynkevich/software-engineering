How to invalidate client-cache in web development. 

![Pasted image 20231014172348](../../../../../_Attachments/Pasted%20image%2020231014172348.png)

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

# References:

1. ! [Browser Caching Best Practices, When to use no-cache vs max-age without breaking your site](https://www.youtube.com/watch?v=z4XdfFscxSk&list=PLQnljOFTspQV1emqxKbcP5esAf4zpqWpe&index=29) (video)
2. ! [HTTP Caching with E-Tags - (Explained by Example)](https://www.youtube.com/watch?v=TgZnpp5wJWU&list=PLQnljOFTspQXOkIpdwjsMlVqkIffdqZ2K&index=62) (video)
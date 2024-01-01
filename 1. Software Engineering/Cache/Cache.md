Caching ***improves application performance*** by ***temporarily storing frequently accessed data in memory, reducing the need to fetch it from slower storage systems***. However, when the original data changes, the cache must be updated or invalidated to maintain data consistency.

![[Pasted image 20230826021400.png]]

- **[[Client-side caching]]** — *The client stores frequently accessed files locally. When a file is requested, the client checks if it has an up-to-date copy and uses it if available, reducing the need to request the file from the server.*
- **Server-side caching** — *The server stores frequently accessed files in memory or on local disks. When a file is requested, the server checks if it’s in the cache and returns it without accessing the disk, reducing disk I/O.*
- **[[Distributed caching]]** — *This involves distributing the cache across multiple servers or nodes. When a file is requested, the system checks if it’s in the cache and returns it from the nearest server, minimizing network traffic.*
- [[CPU level cache]] - *it works completely differently from the lock. and this is the reason why volatile is safe to be used in boolean type, it can guarantee memory visibility, but not automatic operation (still needs lock).*
  
  [[Distributed caching]] is becoming more common due to the decreasing cost of main memory and the increasing speed of network cards. However, cache consistency remains a challenge. Ensuring that the cached data is coherent with the source of truth (e.g., the database) is essential to prevent inconsistencies in application behavior. Different schemes such as “cache-aside” and “read-through cache” are used to manage cache invalidation and consistency.

## References:
1. [[Cache Invalidation]]
2. [[Memcached]]
3. [[Redis]]

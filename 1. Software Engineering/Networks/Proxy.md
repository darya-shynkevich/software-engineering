![[Pasted image 20230826021112.png]]
**Forward Proxy** — It functions as an intermediary between client computers and the internet. It receives requests from client machines and forwards them to internet resources. It enables clients on a private network to access resources on the public internet. A forward proxy provides several essential functions, including:

- It masks the identity of client machines by sending requests on their behalf.
- Forward proxies can cache internet resources, allowing repeated requests for the same resources to be served quickly from the cache, improving performance.
- It regulates the flow of traffic between clients and the internet.
- Forward proxies can log user interactions and requests, providing insights into user behavior.
- Requests and responses can be modified or transformed by the forward proxy.
- It can encrypt traffic between clients and the proxy.

**Reverse proxy** — operates as a mediator between one or more web servers and the internet. Unlike a forward proxy, which forwards requests from clients to the internet, a reverse proxy receives requests from clients on the internet and forwards them to one of the web servers. It then returns the server’s response to the client. The reverse proxy plays a role in various functions, including:

- The actual web servers are shielded from direct exposure to the internet.
- Reverse proxies distribute incoming traffic among multiple backend servers.
- They can mitigate Distributed Denial of Service (DDoS) attacks by filtering and distributing incoming traffic.
- Reverse proxies can modify URLs or content in responses to optimize performance and security.
- By handling SSL/TLS offloading and caching, they improve the responsiveness of backend servers.

# References:

1. ! [The Fundamental Knowledge of System Design — (7) — Proxy](https://interviewnoodle.com/the-fundamental-knowledge-of-system-design-7-c98f76de5e8f)
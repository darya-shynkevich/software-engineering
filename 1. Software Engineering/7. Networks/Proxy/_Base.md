![Pasted image 20230826021112](../../../_Attachments/Pasted%20image%2020230826021112.png)
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

![](../../../_Attachments/Pasted%20image%2020240104090208.png)
# References:

1. ! [The Fundamental Knowledge of System Design — (7) — Proxy](https://interviewnoodle.com/the-fundamental-knowledge-of-system-design-7-c98f76de5e8f)
2. 3. [What is an HTTP Proxy? (Transparent, HTTP and Service Mesh Proxy examples)](https://www.youtube.com/watch?v=x4E4mbobGEc&list=PLQnljOFTspQXOkIpdwjsMlVqkIffdqZ2K&index=46) (video)
4. [Everything you need to know about Proxy Servers - Computer Networking](https://levelup.gitconnected.com/everything-you-need-to-know-about-proxy-servers-computer-networking-50f22cd6986a)
5. [Proxy vs. Reverse Proxy (Explained by Example)](https://www.youtube.com/watch?v=ozhe__GdWC8&list=PLQnljOFTspQXOkIpdwjsMlVqkIffdqZ2K&index=79) (video)
6. [Reverse proxy (web server)](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server) is a web server that centralizes internal services and provides unified interfaces to the public. Requests from clients are forwarded to a server that can fulfill it before the reverse proxy returns the server's response to the client.
7. [Introduction to modern network load balancing and proxying](https://blog.envoyproxy.io/introduction-to-modern-network-load-balancing-and-proxying-a57f6ff80236)
8. [Введение в современную сетевую балансировку и проксирование](https://habr.com/ru/companies/vk/articles/347026/)
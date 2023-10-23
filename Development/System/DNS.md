![[Pasted image 20230826020031.png]]

The Domain Name System (DNS) is a component of the Internet infrastructure that plays the role of translating human-friendly domain names, like “[www.example.com](http://www.example.com/)," into machine-readable IP addresses, such as “192.168.1.1.” 

When you enter a domain name into your web browser’s address bar, the DNS system takes over to find the corresponding IP address for the requested domain. This is essential because computers communicate with each other using IP addresses, and the DNS system facilitates the translation between the domain names that humans use and the IP addresses that computers understand.

The DNS resolution process involves several steps, and it happens “behind the scenes” as you interact with the web.

1. You enter a domain name (e.g., [www.example.com](http://www.example.com/)) into your web browser.
2. Your computer sends a query, known as a recursive DNS query, to a DNS resolver.
3. The DNS resolver is like a librarian who helps find a specific book (IP address) in a library. It checks its cache for previously resolved addresses and, if not found, forwards the query to other DNS servers.
4. The query progresses through a series of DNS servers in a hierarchical manner:

- **Root nameserver:** The first step in the process, similar to an index in a library. It points to more specific locations.
- **Top-level domain (TLD) nameserver:** Further narrows the search by handling specific domain extensions like “.com,” “.org,” etc.
- **Authoritative nameserver:** The final step, where the IP address for the requested domain is found and returned to the DNS resolver.

1. The DNS resolver caches the IP address, so future requests for the same domain can be resolved more quickly.
2. The DNS resolver returns the IP address to your computer, which then uses it to establish a connection with the target server and load the requested content
 The main differentiating point between it and Redis is its lack of data types (**both the key and the value are strings**) and its limited eviction policy of just LRU (least recently used).

Redis is single-threaded while Memcached is **multithreaded**. 

Memcached has a client and server component, each of which is necessary to run the system. The system is designed in a way that half the logic is encompassed in the server, whereas the other half is in the client. However, each server follows the **shared-nothing architecture**. In this architecture, servers are unaware of each other, and there’s no synchronization, data sharing, and communication between the servers.

![](../../../../../../_Attachments/Pasted%20image%2020240127200120.png)



# References:

1. [Memcached Architecture - Crash Course with Docker, Telnet, NodeJS](https://www.youtube.com/watch?v=NCePGsRZFus&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=100) (video)
2. [Introduction to Memcached](http://www.slideshare.net/oemebamo/introduction-to-memcached)
3. [Memcached Interview Questions from Javapoint](https://www.javatpoint.com/memcached-interview-questions-and-answers)
4. [Memcached Interview Questions from Wisdomjobs](https://www.wisdomjobs.com/e-university/memcached-interview-questions.html)
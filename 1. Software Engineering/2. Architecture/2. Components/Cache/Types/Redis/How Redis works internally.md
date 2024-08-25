
> - Do you know what are the key features to make it so fast?
> - Internally it is using [SkipList](../../../../../1.%20Algorithms/Data%20Structures/SkipList.md) with a double list for [LRU](LRU.md), a very efficient data structure which makes it fast
> - Very good, what else? How does it handle incoming connections?
> - It is socket based server, listen and accept the connection then start a thread to process the incoming data”
> - Ok, how about 10k connections at the time? Redis will open 10k threads?

One of the key features of making Redis fast is because its I/O model. As a single-threaded application still fast, which solved the [c10k issue](https://en.wikipedia.org/wiki/C10k_problem#:~:text=The%20C10k%20problem%20is%20the,concurrently%20handling%20ten%20thousand%20connections.) in a smart way — by using the [Epoll model](3.%20IO%20Multiplexing.md).

## [IO Multiplexing](../../../../../6.%20Linux/3.%20IO%20Multiplexing.md)

Redis is like a chef in a computer kitchen, handling one task at a time to ensure perfection. While many helpers might sound good, for Redis, it prefers a single thread for precision, avoiding chaos and ensuring each task is flawlessly executed.

Single-threaded approach:
(+) simplifies the design
(+) commands are executed sequentially
(+) no complexities of managing multiple threads and potential synchronisation issues
(+) can fully utilise the CPU cache, minimising cache misses and optimising performance

 I/O multiplexing allows Redis to monitor multiple connections simultaneously without blocking its main thread. Instead of waiting for data on a single connection, Redis can keep an eye on multiple connections at once.

1. Redis uses the `select()` or `poll()` system call to register interest in multiple sockets (connections) simultaneously.
2. Redis’s single thread invokes the `select()` or `poll()` system call and enters a state of waiting for events. During this time, Redis is not actively processing any specific connection; instead, it awaits notifications about events on the registered sockets and process the requests from the ready sockets one at a time.
3. When an event occurs on any of the registered sockets (e.g., data becomes available for reading), the `select()` or `poll()` call returns. The return value indicates which sockets experienced events, allowing Redis to identify where data is ready to be processed.
4. Redis then proceeds to handle the specific events on the sockets identified by the system call. For example, if data is ready to be read on a particular socket, Redis can initiate the read operation without waiting, addressing the blocking nature of traditional I/O.
5. The event-driven approach is asynchronous; Redis doesn’t actively poll each socket but rather responds to events as they occur. This allows Redis to efficiently manage a large number of connections without wasting resources on constant polling.
6. By waiting for events rather than blocking on individual sockets, Redis maximizes the utilisation of its single thread and system resources. This ensures that the server remains responsive to events across multiple connections without unnecessary delays.

# References:

1. ~~[What is Redis and how does it work Internally](https://medium.com/@ayushsaxena823/what-is-redis-and-how-does-it-work-cfe2853eb9a9)~~


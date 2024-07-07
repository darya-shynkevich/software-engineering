An event loop is a loop that can register tasks to be executed, execute them, delay or even cancel them and handle different events related to these operations.
Generally, we schedule multiple async functions to the event loop. The loop runs one function, while that function waits for IO, it pauses it and runs another. When the first function completes IO, it is resumed. Thus two or more functions can co-operatively run together. This the main goal of an event loop.

How event loop was born: [Socket](4.%20Socket.md)

##### Event loop in browser (Chrome)

![Pasted image 20231205112239](Pasted%20image%2020231205112239.png)

1. Chrome has a multi-process design
2. Each process community each other through event loops. (sounds familiar? yes. In a distributed or Microservice designed system, they talk with each other through a message queue)
3. Each process sends messages to different queues: Network, UI, Timer, and Render queues; in each queue, process the request and send back the callback result.
4. Chrome is a desktop app and its core design is using distributed system and microservice thinking.

##### Event loop in OS (Simplified model)

![Pasted image 20231205112333](Pasted%20image%2020231205112333.png)

1. The win-form application started. UI-Thread which is the main thread focuses on rendering the UI elements
2. The user clicks the mouse on a button, and the win-form application “publishes” a message to the OS message queue to “subscribe” mouse (or keyboard events)
3. Later on, when an I/O device like a mouse or keyboard gets input and then publishes the data into the OS message queue, the OS will check the “subscribers” (or observers) and dispatch the events to them, this is the “callback” step
4. The Windows application processes the event
5. Above is a simplified flow of how the message is being dispatched in OS, only to demonstrate the idea.
6. Yes, other OS like Android or IOS also have a message queue. to keep the post short, will not dive into the implementation details.

##### Event loop in nginx

![Pasted image 20231205112442](Pasted%20image%2020231205112442.png)

1. When nginx server starts It will fork multiple worker processes
2. In each worker process, there is an event loop, which is the “epoll model” and here is what happens when a worker process is forked by the master process:
3. OS created the Epoll Red-Black tree.
4. The user process adds one or more socket file descriptors into the R-B tree by calling epoll APIs. then listen to the waitlist queue for new data coming.
5. When the http request comes from the network driver. It finds the file descriptor based on the socket port and puts the File descriptor into the waitlist queue.
6. The user process got the data with FileDescritor info from the waitlist queue.

#### Event loop in System level

At the system level, we call it a “Message queue”. the problems we are solving are(similar to event loop):
- Decouple
- Scale
- Async process and nonblocking
- Higher throughput
- Glue systems

![Pasted image 20231205112634](Pasted%20image%2020231205112634.png)

The event-loop is an idea to enable non-blocking I/O, improved system throughput; decouple, scale, and integration; the implementation doesn’t have to be looping (e.g. epoll, message queue).
# References:

1. [Python asyncio: Future, Task and the Event Loop](!https://masnun.com/2015/11/20/python-asyncio-future-task-and-the-event-loop.html)
2. [“do you know event loop?” Asked by my interviewer](!https://iorilan.medium.com/do-you-know-event-loop-asked-by-my-interviewer-19d270a246c8)
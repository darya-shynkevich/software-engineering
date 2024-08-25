
- _[[Multithreading]]_
- _[[Multiprocessing]]_
- _[[Asyncio]]_

# Quick Recap

1. _Sync: Blocking operations,  the tasks are executed in sync, one after one
2. _Async: Independant and Non blocking operations, tasks may start and complete independent of each other_
3. _Concurrency: Making progress together._
4. _Parallelism: Making progress in parallel._

Example: 
Call to agent and then write to boss => sync
Call to agent and write to boss => concurrency
Ask friend to call to agent and write to boss by yourself => parallelism

*Parallelism is in fact a form of concurrency. But parallelism is hardware dependent. For example if there’s only one core in the CPU, two operations can’t really run in parallel. They just share time slices from the same core. This is concurrency but not parallelism. But when we have multiple cores, we can actually run two or more operations (depending on the number of cores) in parallel.*

_**When to use Multiprocessing over Multithreading ?**_

1. [_multi-threading in python: is it really performance effiicient most of the time?_](https://stackoverflow.com/questions/24744739/multi-threading-in-python-is-it-really-performance-effiicient-most-of-the-time)
2. [_What's the difference between ThreadPool vs Pool in the multiprocessing module?_](https://stackoverflow.com/questions/46045956/whats-the-difference-between-threadpool-vs-pool-in-the-multiprocessing-module)

```Python
if io_bound:
    if io_very_slow:
        print("Use Asyncio")
    else:
        print("Use Threads")
else:
    print("Multi Processing")


- CPU Bound => Multi Processing
- I/O Bound, Fast I/O, Limited Number of Connections => Multi Threading
- I/O Bound, Slow I/O, Many connections => Asyncio
```

_"If your code is IO bound, both **multiprocessing** and **multithreading** in Python will work for you. Multiprocessing is a easier to just drop in than threading but has a higher memory overhead. If your code is CPU bound, multiprocessing is most likely going to be the better choice — especially if the target machine has multiple cores or CPUs. For web applications, and when you need to scale the work across multiple machines, **RQ library** is going to be better for you."_

![Pasted image 20231204230352](Pasted%20image%2020231204230352.png)

# [Multiprocessing](2.%20Multiprocessing.md)

- **Process** - an independent execution unit that the OS manages.
- It has memory space and resources like CPU time and I/O devices.
- Identified by a unique Process Identifier (PID), processes can run concurrently and be managed by the operating system to share resources efficiently.
-  It has its own independent virtual address space, which includes code, data, and stack segments.
- It communicates with other processes through Inter-Process Communication (IPC) mechanisms like pipes, message queues, and shared memory.
- It requires more resources to create and manage. Each process needs its own memory space and OS resources. Higher overhead due to the need for separate memory space and additional resources.
- Shared memory allows processes to communicate by accessing a common memory space. This enables direct reading and writing of data but requires careful synchronisation to avoid race conditions.

To get parallelism, Python introduced the `multiprocessing` module which provides APIs which will feel very similar if you have used Threading before.

Processes are costly to spawn. So for I/O, Threads are chosen largely.

# [Multithreading](1.%20Multithreading.md)

- Thread is the smallest unit of execution within a process, sharing its memory space.
- It shares the process’s virtual address space but has its stack => it requires fewer resources.
- Threads communicate more easily by sharing data within the process.
- Multitreading allows a process to execute multiple threads concurrently. Each thread represents a separate execution path within the same program, sharing the same memory space but operating independently.

A multithreaded process uses an operating system API to create, manage, and terminate threads. Threads run concurrently, with the OS scheduler allocating CPU time slices to each. They communicate via shared memory and use synchronization mechanisms like mutexes and semaphores to coordinate access to shared resources.
- **Main thread** — assign tasks to your workers (threads).
- **Workers (threads)** — perform the tasks concurrently, sharing the memory and resources.
- When multiple workers need the same memory and resources, leading to potential delays.
-  Methods to manage conflicts, such as waiting for turns or notifying when a resource is free.

*Consider a web browser — it’s a multi-threaded application. When we open multiple tabs, each tab is handled by a separate thread. So, while one tab is loading a webpage, we can continue scrolling or reading on another. This is **multithreading** at work!  <- how threads work in normal languages*

## Thread Synchronization

1. **Locks:** These are the most basic and essential synchronization technique. A lock allows only one thread to enter the part that’s locked, ensuring mutual exclusivity. Other threads attempting to enter the locked code will wait until the lock is released.
2. **Semaphores:** A semaphore restricts the number of simultaneous accesses to a shared resource. It’s like a nightclub with a certain capacity. If the capacity is reached, new people can enter only when someone inside leaves.

## Thread Pooling

Creating and destroying threads requires a significant amount of CPU time and resources. To mitigate this, we use thread pools, a group of pre-instantiated, idle threads that stand ready to be used. This way, when a task arrives, it can be directly allocated to one of the idle threads, saving the overhead of thread creation.

`ThreadPoolExecutor` 

Thread pools solve the problem of uncontrolled thread creation. Instead of submitting each task to a separate thread, we submit tasks to a queue and let a group of threads, called a **thread pool**, take and process the tasks from the queue. We predefine the maximum number of threads in a thread pool, so the server cannot start too many of them.

## [GIL](16.%20GIL.md)

The GIL is a locking mechanism that the Python interpreter runs only one thread at a time. That is only one thread can execute Python byte code at any given time. This GIL makes sure that multiple threads **DO NOT** run in parallel.

Quick facts about the GIL:
- One thread can run at a time.
- The Python Interpreter switches between threads to allow concurrency.
- The GIL is only applicable to CPython (the defacto implementation). Other implementations like Jython, IronPython don’t have GIL.
- GIL makes single threaded programs fast.
- For I/O bound operations, GIL usually doesn’t harm much.
- GIL makes it easy to integrate non thread safe C libraries, thansk to the GIL, we have many high performance extensions/modules written in C.
- For CPU bound tasks, the interpreter checks between `N` ticks and switches threads. So one thread does not block others.

## Best Practices

1. **Minimize Thread Usage:** Threads are expensive to create and destroy, and too many threads can lead to a lot of context switching, reducing the overall efficiency of the system. So, use them sparingly and wisely.
2. **Use Thread Pools:** A thread pool is a group of pre-instantiated, idle threads that are ready to be used. Using a thread pool minimizes the overhead of thread creation and destruction.
3. **Avoid Thread Priorities:** Depending on thread priorities for program correctness can lead to portability issues, as thread scheduling is not consistent across different operating systems.
4. **Synchronize Carefully:** Incorrect synchronization can lead to problems like deadlocks and race conditions. It’s often better to use higher-level synchronization utilities provided by your language’s standard library, rather than trying to solve these problems yourself with low-level primitives.
# [Concurrency](3.%20Concurrency.md)

Concurrency means multiple computations are happening in overlapping time periods. *It’s like juggling. Even though you’re managing multiple balls, you’re only ever handling one at a time.* By this mechanism, an OS can run thousands of processes on a machine that has only a few cores. If multiple tasks do run at the same physical time, as in the case of a multi-core machine or a cluster, then we have **parallelism**, a special case of concurrency.

Processes are costly to spawn. So for I/O, Threads are chosen largely. We know that I/O depends on external stuff - slow disks or nasty network lags ***make I/O often unpredictable***. Now, let’s assume that we are using threads for I/O bound operations. 3 threads are doing different I/O tasks. ***The interpreter would need to switch between the concurrent threads and give each of them some time in turns.*** Let’s call the threads - `T1`, `T2` and `T3`. The three threads have started their I/O operation. `T3` completes it first. `T2` and `T1` are still waiting for I/O. The Python interpreter switches to `T1` but it’s still waiting. Fine, so it moves to `T2`, it’s still waiting and then it moves to `T3` which is ready and executes the code. Do you see the problem here?

`T3` was ready but the interpreter switched between `T2` and `T1` first - that incurred switching costs which we could have avoided if the interpreter first moved to `T3`, right?

Asyncio provides us an event loop along with other good stuff. The ***event loop tracks different I/O events and switches to tasks which are ready and pauses the ones which are waiting on I/O.*** Thus we don’t waste time on tasks which are not ready to run right now.

The idea is very simple. There’s an event loop. And we have functions that run async, I/O operations. We give our functions to the event loop and ask it to run those for us. The event loop gives us back a `Future` object, it’s like a promise that we will get something back in the _future_. We hold on to the promise, time to time check if it has a value (when we feel impatient) and finally when the future has a value, we use it in some other operations.

[asyncio](asyncio)

[How event loop works in Python](How%20event%20loop%20works%20in%20Python.md)

![Pasted image 20231204233400](Pasted%20image%2020231204233400.png)

## Benefits

1. **Improved Performance:** Concurrency allows for the overlapping of input/output operations with computations, which can lead to significant performance gains, especially in I/O intensive programs.
2. **Faster Response Time:** In interactive systems, concurrency can make the system more responsive. While a long-running task is executing, the system can still respond to other inputs.
3. **Better Resource Utilization:** Concurrency enables better utilization of resources by allowing tasks to run whenever the resources they need are available.

## Concurrency issues

1. **Race Conditions:** Race conditions occur when the output is dependent on the sequence or timing of other uncontrollable events. It’s like two threads racing to write a value to a shared variable.
2. **Deadlocks:** Deadlocks occur when two or more tasks are unable to proceed because each is waiting for the other to release a resource.
3. **Starvation:** Starvation is a condition where a thread is unable to gain regular access to shared resources and is unable to make progress.

# Concurrency vs. Parallelism

![Pasted image 20231204230318](Pasted%20image%2020231204230318.png)

![Pasted image 20231205000018](Pasted%20image%2020231205000018.png)

# References:

1. ~~[Process vs Thread](https://jinlow.medium.com/process-vs-thread-77a964da82b4)~~
2. [Modern operating systems. Chapter 2](http://libgen.rs/book/index.php?md5=AC5BFB29F9BC58C7EEBF11F627221EA0) (book)
3. [Multithreading and Concurrency Concepts I Wish I Knew Before the Interview](https://levelup.gitconnected.com/multithreading-and-concurrency-concepts-i-wish-i-knew-before-the-interview-11895226179)
4. [Parallelism implies Concurrency. But Concurrency doesn’t always mean Parallelism.](http://masnun.rocks/2016/10/06/async-python-the-different-forms-of-concurrency/)
5. **[Multithreading and concurrency fundamentals](https://learningdaily.dev/multithreading-and-concurrency-fundamentals-fb40a502fdb)**
6. **[Asynchronous vs Multithreading and Multiprocessing Programming (The Main Difference)](https://www.youtube.com/watch?v=0vFgKr5bjWI&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=5)**
7. [**Speed Up Your Python Program With Concurrency**](https://realpython.com/python-concurrency/)
8. **[Multiprocessing VS Threading VS AsyncIO in Python](https://leimao.github.io/blog/Python-Concurrency-High-Level/)**
9. [I/O is no longer the bottleneck](https://benhoyt.com/writings/io-is-no-longer-the-bottleneck/)
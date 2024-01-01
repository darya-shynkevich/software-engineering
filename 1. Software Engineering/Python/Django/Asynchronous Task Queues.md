An **asynchronous task queue** is one where tasks are executed at a different time from when they are created, and possibly not in the same order they were created. 

**Broker:** The storage for the tasks themselves. This can be implemented using any sort of persistence tool, although in Django the most common ones in use are RabbitMQ and Redis. In the human-powered example, the storage is an online issue tracker.
**Producer:** The code that adds tasks to the queue to be executed later. This is application code, the stuff that makes up a Django project.
**Worker:** The code that takes tasks from the broker and performs them. Usually there is more than one worker. Most commonly each worker runs as a daemon under supervision.

*Arguably it comes down to whether a particular piece of code causes a bottleneck and can be delayed for later when more free CPU cycles are available.*

**Results take time to process:** Task queue should probably be used.
**Users can and should see results immediately:** Task queue should not be used.

![[Pasted image 20231203220348.png]]

## Choosing Task Queue Software

![[Pasted image 20231203220439.png]]

1. If time permits, move all asynchronous processes to Serverless systems such as AWS Lambda.
2. If API calls to Serverless become an issue, encapsulate these calls in [[Celery]] tasks. For us, this has only been a problem with bulk API calls to AWS Lambda.
3. Use Django Channels for websockets. The lack of retry mechanism forces you to invent things that Celery provides out-of-the-box.
4. For security and performance reasons, any and all API calls to user-defined URLs are done through task queues.

## Best Practices for Task Queues

#### 1. Treat Tasks Like Views

All task queue packages do some kind of serialization/abstraction of our task functions and their arguments. This makes debugging them much more difficult. By using our task func- tions to call more easily tested normal functions, we not only make writing and debugging our code easier, we also encourage more reuse.
#### 2. Tasks Aren’t Free

Even if resource-intensive code is executed from a task, it should still be written as cleanly as possible, minimizing any unnecessary resource usage.

#### 3. Only Pass JSON-Serializable Values to Task Functions

Just like views, for task function arguments, only pass JSON-serializable values. That limits us to integers, floats, strings, lists, tuples, and dictionaries. Don’t pass in complex objects.
1. Passing in an object representing persistent data. For example, ORM instances can cause a race condition. This is when the underlying persistent data changes before the task is run. Instead, pass in a primary key or some other identifier that can be used to call fresh data.
2. Passing in complex objects that have to be serialized into the task queue is time and memory consuming. This is counter-productive to the benefits we’re trying to achieve by using a task queue.
3. Easier in debugging
4. Depending on the task queue in use, only JSON-serializable primitives are accepted.

#### 4. Write Tasks as Idempotent Whenever Possible

Idempotent => you can run the task multiple times and get the same result. Even better to use pure functions (without side effects).

#### 5. Don’t Keep Important Data in Your Queue

The solution is to track the status of an action within the affected record(s). For example, as a customer is about to be billed, mark them as not having been billed yet, then call the task. If the task succeeds, have it update the customer has having been billed. If the task fails, then it will fail to update the customer and a simple query will reveal the customer hasn’t yet paid their bill.

#### 6. Learn How to Monitor Tasks and Workers

#### 7. Logging!

#### 8. Monitor the Backlog

As traffic increases, tasks can pile up if there aren’t enough workers. When we see this happening, it’s time to increase the number of workers.

#### 9. Ignore Results We Don’t Need

#### 10. Use the Queue’s Error Handling

1. Max retries for a task
2. Retry delays

*Retry delays deserve a lot of consideration. When a task fails, we like to wait at least 10 seconds before trying again. Even better, if the task queue software allows it, increase the delay each time an attempt is made. We set things this way in order to give the conditions that caused a failure to resolve themselves.*




So in the above coroutine model, as you can see, there is only one “waiter” thread. you may wonder why not have multiple “waiters” to speed it up. yes, this is what Goroutine is improving.

![Pasted image 20231015215247](../../../_Attachments/Pasted%20image%2020231015215247.png)

- So there are 2 waiters. which is a combination of “M” and “P” in `GMP`.
- As you can see the good feature of the model is, once “your order” is finished, even if “F2” is queuing under another waiter, the first waiter can still serve F2 order instead of leaving F2 blocked by F1 (`work stealing`)
- Another case is as below. let’s say some thread called “sleep” (your order need to wait 20mins). So the “waiter”(P+M) could be off from this task and serve the F2’s order (`hands off`) immediately.

![Pasted image 20231015215425](../../../_Attachments/Pasted%20image%2020231015215425.png)

Goroutine(Golang’s coroutine) is a coroutine built-in Golang which improved the traditional coroutine using:

- Golang implemented built-in logical processors and threads, plus a complete scheduling mechanism at the language level.
- There is a “`global queue`” and multiple logical “`routine queues`”, making it able to handle high concurrency naturally.
- The routine is lightweight (2kb per routine).
- “`Work steals`” and “`Handoff processor`” made it “smarter”.


When we work in multi-threading code, to ensure thread safety(automatic operation), we use locks or synchronized keywords. under the hood, it uses semaphore or mutex.

![Pasted image 20231014200638](../../_Attachments/Pasted%20image%2020231014200638.png)

So at the CPU level, there are multi-level caches. L1 and L2 are within “each CPU world”. they have a shared L3 cache, and yes, Memory is hit when the cache is missed in all 3 levels.

1. `Thread1` updates the variable `flag=1`
2. And very quickly at the same time when `thread2` tries to read the value, it gets `flag=0`. because CPU2’s L1 or L2 cache is hit which stores the old value.

So we already know that the lock can make it work. but it is not cheap, what if we only have a boolean variable like ‘flag=true/false’? Is there a cheaper way to make sure this variable is “VISIBLE” to other threads? yes, that’s where the ‘volatile’ keyword comes into play.

# How volatile exactly works

![Pasted image 20231014200845](../../_Attachments/Pasted%20image%2020231014200845.png)

The above picture has a couple of steps but just 3 key points.

- **Cache Coherence protocol e.g. MESI.** it depends on CPU architecture, there are different cache coherence protocols implemented. so in step 3 above, after the volatile variable is updated, the CPU will “tell” other CPUs that this ‘flag’ variable’s value is updated, pls directly read from memory and update “your cache”.
- **Snooping (Bus-based coherence)**. Snooping is a mechanism used in bus-based cache coherence protocols (e.g., MESI) where each CPU monitors the system bus for changes to memory locations; When a CPU writes to a memory location, other CPUs snoop the bus to detect changes and update their caches accordingly; This mechanism helps maintain cache coherence by ensuring that all CPUs have a consistent view of memory.
- **Memory barrier**. for languages like Java or C#, there is JIT (just in time) compiler. this command is to “tell” the compiler that “before and after the read or write this volatile variable, do not insert any instructions”. to make sure the read/write is on the main memory.


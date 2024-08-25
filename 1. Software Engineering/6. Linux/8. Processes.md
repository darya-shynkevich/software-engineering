A process - an instance of a running program. This process includes memory needed by the program and associated resources.

Process Control Block is a data structure maintained by the Operating System for every process. The PCB is identified by an integer process ID (PID). A PCB keeps all the information needed to keep track of a process and resume it during multitasking.

The process is the abstraction around program execution so the CPU can know where it left off. You would obviously want some isolation to correctly reason about the state and resources attached to these processes. Since a user starts a process, following Unix principles, the process has the same rights and privileges as the user who created the process.

So processes are said to _**own**_ the following resources:
1. Memory
2. Operating Descriptors (files)
3. Security attributes (permissions)
4. Overall process context.
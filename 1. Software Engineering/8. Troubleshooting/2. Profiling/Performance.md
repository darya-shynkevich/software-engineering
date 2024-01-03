**Benchmarking** is where you time your code to see how long it takes to run. You can use Python's built-in `time` module, as we'll show later, or use more sophisticated tools like [timeit](https://docs.python.org/3/library/timeit.html).

**Profiling**, on the other hand, provides a more detailed view of your code's execution. It shows you where your code spends most of its time, which functions are called, and how often. Python's built-in [profile](https://docs.python.org/3/library/profile.html) or [cProfile](https://docs.python.org/3/library/cProfile.html) modules can be used for this.

! **Note:** Profiling can add quite a bit of overhead to your code execution, so the execution time shown by the profiler will likely be longer than the actual execution time.

[Why does Python Code Run Faster in a Function?](../../4.%20Python/Why%20does%20Python%20Code%20Run%20Faster%20in%20a%20Function?.md)
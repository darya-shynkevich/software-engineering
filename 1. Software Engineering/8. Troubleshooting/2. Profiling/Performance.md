**Benchmarking** is where you time your code to see how long it takes to run. You can use Python's built-in `time` module, as we'll show later, or use more sophisticated tools like [timeit](https://docs.python.org/3/library/timeit.html).

**Profiling**, on the other hand, provides a more detailed view of your code's execution. It shows you where your code spends most of its time, which functions are called, and how often. Python's built-in [profile](https://docs.python.org/3/library/profile.html) or [cProfile](https://docs.python.org/3/library/cProfile.html) modules can be used for this.

! **Note:** Profiling can add quite a bit of overhead to your code execution, so the execution time shown by the profiler will likely be longer than the actual execution time.

[Why does Python Code Run Faster in a Function?](../../4.%20Python/Why%20does%20Python%20Code%20Run%20Faster%20in%20a%20Function?.md)

[**Performance Optimization by Brotli Compression](https://blogs.akamai.com/2016/02/understanding-brotlis-potential.html):**
1. [Boosting Site Speed Using Brotli Compression at LinkedIn](https://engineering.linkedin.com/blog/2017/05/boosting-site-speed-using-brotli-compression)
2. [Brotli at Booking.com](https://medium.com/booking-com-development/bookings-journey-with-brotli-978b249d34f3)
3. [Brotli at Treebo](https://tech.treebo.com/a-tale-of-brotli-compression-bcb071d9780a)
4. [Deploying Brotli for Static Content at Dropbox](https://dropbox.tech/infrastructure/deploying-brotli-for-static-content)
5. [Progressive Enhancement with Brotli at Yelp](https://engineeringblog.yelp.com/2017/07/progressive-enhancement-with-brotli.html)
6. [Speeding Up Redis with Compression at Doordash](https://doordash.engineering/2019/01/02/speeding-up-redis-with-compression/)

# References:

1. [Performance is a Feature](https://blog.codinghorror.com/performance-is-a-feature/)
2. [Make Performance Part of Your Workflow](https://codeascraft.com/2014/12/11/make-performance-part-of-your-workflow/)
3. [The Benefits of Server Side Rendering over Client Side Rendering](https://medium.com/walmartlabs/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8)
4. **[Discovering Backend Bottlenecks: Unlocking Peak Performance](https://www.udemy.com/course/discovering-backend-bottlenecks-unlocking-peak-performance/)** (course)
5. **[Best practices for dealing with slow performance](https://forum.djangoproject.com/t/best-practices-for-dealing-with-slow-performance/23707)**
6. [How CPU Efficient is your App](https://www.youtube.com/watch?v=BTD5I1BMx2Q) (video)
7. [I ask this question to every Backend Engineer I interview](https://www.youtube.com/watch?v=bDIB2eIzIC8&list=PLQnljOFTspQXawSLc0ohr9qcai8AVcAH5&index=2) (video)
8. [Profiling in Python: How to Find Performance Bottlenecks](https://realpython.com/python-profiling/)
9. [How I Redesigned The Backend To Quickly Handle Millions Of Reads (And Writes)](https://blog.bitsrc.io/how-i-redesigned-the-backend-to-quickly-handle-millions-of-reads-and-writes-58cfe989e6f8)
10. [Choosing a Language Stack at WeWork](https://engineering.wework.com/choosing-a-language-stack-cac3726928f6)
11. [Python at Netflix](https://netflixtechblog.com/python-at-netflix-bba45dae649e)
12. [Python at scale (3 parts) at Instagram](https://instagram-engineering.com/python-at-scale-strict-modules-c0bb9245c834)
13. [5 Ways to Measure Execution Time in Python](https://superfastpython.com/benchmark-execution-time/)
14. [An Overview of Profiling Tools for Python](https://www.blog.pythonlibrary.org/2020/04/14/an-overview-of-profiling-tools-for-python/)
15. [A Guide to Analyzing Python Performance](https://everyhue.me/posts/python-performance-analysis/)
16. [Профилирование Python-программ и анализ их производительности](https://habr.com/ru/company/wunderfund/blog/656571/)
17. [Честное перформанс-тестирование / Дмитрий Пивоваров (ZeroTurnaround)](https://www.youtube.com/watch?v=8Mzs3arFGZo) (video)
18. [Алексей Кузьмин, ДомКлик «Поиск и оптимизация узких мест в Python»](https://www.youtube.com/watch?v=tDZHhIiACDA)
19. [Python Performance Profiling & Optimization](https://monadical.com/posts/python-performance-profiling.html)
20. [Краткое содержание доклада Василия Литвинова (Производительность кода на Python — инструменты оптимизации)](https://vk.com/wall-105242702_916)
21. [Оптимизация кода на Python с помощью ctypes](https://habr.com/ru/company/otus/blog/490244/)
22. [Is Python Really a Bottleneck?](https://towardsdatascience.com/is-python-really-a-bottleneck-786d063e2921)
23. [Честное перформанс-тестирование](https://www.youtube.com/watch?v=8Mzs3arFGZo) (video)
24. [Приемы оптимизации веб-графики в 2021 году](https://habr.com/ru/company/domclick/blog/580850/)
25. [How we optimized Python API server code 100x](https://towardsdatascience.com/how-we-optimized-python-api-server-code-100x-9da94aa883c5)
26. [How vectorization speeds up your Python code](https://pythonspeed.com/articles/vectorization-python/)
27. [Ускорение производительности Python в 3.11](https://habr.com/ru/post/662087/)
28. [Шпаргалка по метрикам производительности cURL: как измерить задержку сервера](https://habr.com/ru/company/ruvds/blog/568614/)
29. [Профилирование Python-программ и анализ их производительности](https://habr.com/ru/companies/wunderfund/articles/656571/)
30. [Python Performance Optimization](https://stackabuse.com/python-performance-optimization/)
31. [Profiling Python with cProfile, and a speedup tip](https://www.wrighters.io/profiling-python-with-cprofile-and-a-speedup-tip/)
32. [Taichi и 100-кратное ускорение Python-кода](https://habr.com/ru/companies/wunderfund/articles/688134/)
33. [Big List Of 20 Common Bottlenecks](http://highscalability.com/blog/2012/5/16/big-list-of-20-common-bottlenecks.html)
34. [7 Tips to Optimize Your Backend API Without Caching](https://www.youtube.com/watch?v=R4NvQMF58K4&list=PLQnljOFTspQWGuRmwojJ6LiV0ejm6eOcs&index=6) (video)
35. [Identifying backend connection latencies with chrome devtools](https://www.youtube.com/watch?v=IjJtmfjp1Ls&list=PLQnljOFTspQUybacGRk1b_p13dgI-SmcZ&index=49) (video)
36. [DevTools Waterfall Deep Dive - Diagnose Your Backend and Improve the Frontend User Experience](https://www.youtube.com/watch?v=6TEwVDNA7bI&list=PLQnljOFTspQWGuRmwojJ6LiV0ejm6eOcs&index=26) (video)
37. [Compression Techniques to Solve Network I/O Bottlenecks at eBay](https://www.ebayinc.com/stories/blogs/tech/how-ebays-shopping-cart-used-compression-techniques-to-solve-network-io-bottlenecks/)
38. [Optimizing Web Servers for High Throughput and Low Latency at Dropbox](https://blogs.dropbox.com/tech/2017/09/optimizing-web-servers-for-high-throughput-and-low-latency/)
39. [Linux Performance Analysis in 60.000 Milliseconds at Netflix](https://medium.com/netflix-techblog/linux-performance-analysis-in-60-000-milliseconds-accc10403c55)
40. [Decreasing RAM Usage by 40% Using jemalloc with Python & Celery at Zapier](https://zapier.com/engineering/celery-python-jemalloc/)
41. [Optimizing APIs at Netflix](https://medium.com/netflix-techblog/optimizing-the-netflix-api-5c9ac715cf19)
42. [Performance Tuning on Quartz Scheduler at eBay](https://www.ebayinc.com/stories/blogs/tech/performance-tuning-on-quartz-scheduler/)
43. [API Profiling at Pinterest](https://medium.com/@Pinterest_Engineering/api-profiling-at-pinterest-6fa9333b4961)
44. [Improving HDFS I/O Utilization for Efficiency at Uber](https://eng.uber.com/improving-hdfs-i-o-utilization-for-efficiency/)
45. [Unthrottled: Fixing CPU Limits in the Cloud (2 parts) at Indeed](https://engineering.indeedblog.com/blog/2019/12/unthrottled-fixing-cpu-limits-in-the-cloud/)
46. **[Dive into API Performance Testing with Locust](https://medium.com/@abhishekranjandev/dive-into-api-performance-testing-with-locust-78c2ef6b8f1d)**
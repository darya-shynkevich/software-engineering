
## **Performance vs scalability** 

A service is **scalable** if it results in increased **performance** in a manner proportional to resources added. Generally, increasing performance means serving more units of work, but it can also be to handle larger units of work, such as when datasets grow.

Another way to look at performance vs scalability:

1. If you have a **performance** problem, your system is slow for a single user.
2. If you have a **scalability** problem, your system is fast for a single user but slow under heavy load.
3. **Source(s) and further reading:**
    1. [A word on scalability](http://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html)
    2. [Scalability, availability, stability, patterns](http://www.slideshare.net/jboner/scalability-availability-stability-patterns/)

## **[[Latency]] vs throughput:**

1. **[[Latency]]** is the time to perform some action or to produce some result.
2. **Throughput** is the number of such actions or results per unit of time.
3. Generally, you should aim for **maximal throughput** with **acceptable latency**.
    1. [Understanding latency vs throughput](https://community.cadence.com/cadence_blogs_8/b/fv/posts/understanding-latency-vs-throughput)

# References:

1. [Scalability Lecture at Harvard](https://www.youtube.com/watch?v=-W9F__D3oY4) (video). Topics covered: Vertical scaling, Horizontal scaling, Caching, Load balancing, Database replication, Database partitioning
2. [Scalability](https://web.archive.org/web/20221030091841/http://www.lecloud.net/tagged/scalability/chrono) Topics covered: [Clones,](https://web.archive.org/web/20220530193911/https://www.lecloud.net/post/7295452622/scalability-for-dummies-part-1-clones) [Databases,](https://web.archive.org/web/20220602114024/https://www.lecloud.net/post/7994751381/scalability-for-dummies-part-2-database) [Caches](https://web.archive.org/web/20230126233752/https://www.lecloud.net/post/9246290032/scalability-for-dummies-part-3-cache), [Asynchronism](https://web.archive.org/web/20220926171507/https://www.lecloud.net/post/9699762917/scalability-for-dummies-part-4-asynchronism)
3. [Learn How To Think At Scale](http://highscalability.com/blog/2009/7/30/learn-how-to-think-at-scale.html)
4. [The Art of Architecting for Scale - David Laitner, SQUER Solutions | Craft Conference, 2023](https://www.youtube.com/watch?v=qfvPUE79wlg&list=PLcTa2e7_ENN_ZmTmGC_AFh1ArFgdEb5Z6&index=62) (video)
5. [Introduction to architecting systems for scale.](https://lethain.com/introduction-to-architecting-systems-for-scale/)
6. [Intuitively Showing How To Scale A Web Application Using A Coffee Shop As An Example](http://highscalability.com/blog/2014/3/17/intuitively-showing-how-to-scale-a-web-application-using-a-c.html)
7. [A Flock Of Tasty Sources On How To Start Learning High Scalability](http://highscalability.com/blog/2014/11/24/a-flock-of-tasty-sources-on-how-to-start-learning-high-scala.html)
8. [Scaling CPU-intensive Backends - The Backend Engineering Show](https://www.youtube.com/watch?v=hEap0fIwaGI&list=PLQnljOFTspQUybacGRk1b_p13dgI-SmcZ&index=42) (video)
9. [Great Introductory Video On Scalability From Harvard Computer Science](http://highscalability.com/blog/2010/11/24/great-introductory-video-on-scalability-from-harvard-compute.html)
10. [Real World Web: Performance & Scalability](http://highscalability.com/blog/2009/8/18/real-world-web-performance-scalability.html)
11. [Distributed System: 50 Scaling Principles](https://medium.com/@bindubc/distributed-system-50-scaling-principles-2de52521e888)
12. [8 Commonly Used Scalable System Design Patterns](http://highscalability.com/blog/2010/12/1/8-commonly-used-scalable-system-design-patterns.html)
13. [The 7 Stages Of Scaling Web Apps](http://highscalability.com/7-stages-scaling-web-apps)
14. [Five Ways To Stop Framework Fixation From Crashing Your Scaling Strategy](http://highscalability.com/five-ways-stop-framework-fixation-crashing-your-scaling-strategy)
15. [What's Your Scalability Plan?](http://highscalability.com/blog/2008/2/13/whats-your-scalability-plan.html)
16. [8 Commonly Used Scalable System Design Patterns](http://highscalability.com/blog/2010/12/1/8-commonly-used-scalable-system-design-patterns.html)
17. [7 Scaling Strategies Facebook Used To Grow To 500 Million Users](http://highscalability.com/blog/2010/8/2/7-scaling-strategies-facebook-used-to-grow-to-500-million-us.html)
18. [7 Design Patterns For Almost-Infinite Scalability](http://highscalability.com/blog/2010/12/16/7-design-patterns-for-almost-infinite-scalability.html)
19. [The 7 Stages Of Scaling Web Apps](http://highscalability.com/blog/2008/9/23/the-7-stages-of-scaling-web-apps.html)
20. [6 Ways To Kill Your Servers - Learning How To Scale The Hard Way](http://highscalability.com/blog/2010/8/23/6-ways-to-kill-your-servers-learning-how-to-scale-the-hard-w.html)
21. [System Design: When to Choose Horizontal vs Vertical Scaling?](https://medium.com/ayokoding/system-design-when-to-choose-horizontal-vs-vertical-scaling-a96877e43ca7)
22. [Scalability Worst Practices](https://www.infoq.com/articles/scalability-worst-practices)
23. [Design for Scaling Out](https://docs.microsoft.com/en-us/azure/architecture/guide/design-principles/scale-out)
24. [Design for Evolution](https://docs.microsoft.com/en-us/azure/architecture/guide/design-principles/design-for-evolution)
25. [The 4 Building Blocks Of Architecting Systems For Scale](http://highscalability.com/blog/2012/9/19/the-4-building-blocks-of-architecting-systems-for-scale.html)
26. [Scale like a Pro](https://medium.com/swlh/how-i-scaled-a-software-systems-performance-by-35-000-6dacd63732df)
27. [A Perfect Fifth Of Notes On Scalability](http://highscalability.com/blog/2012/1/10/a-perfect-fifth-of-notes-on-scalability.html)
28. [Architecture Issues When Scaling Web Applications: Bottlenecks, Database, CPU, IO](http://highscalability.com/blog/2014/5/12/4-architecture-issues-when-scaling-web-applications-bottlene.html)
29. [Stateless vs Stateful Scalability](http://ithare.com/scaling-stateful-objects/)
30. [Scale Up vs Scale Out](https://www.brianjgraf.com/scalability-scale-up-scale-out-care/)
31. [Scale Up vs Scale Out: Hidden Costs](https://blog.codinghorror.com/scaling-up-vs-scaling-out-hidden-costs/)
32. [Scalable Agreement - Towards Ordering as a Service](https://www.usenix.org/legacy/event/hotdep10/tech/full_papers/Kapritsos.pdf)
33. [Scalable Eventually Consistent Counters over Unreliable Networks](https://arxiv.org/pdf/1307.3207v1.pdf) - Scalable counting is tough in an unreliable world*
34. [Jeff Dean On Large-Scale Deep Learning At Google](http://highscalability.com/blog/2016/3/16/jeff-dean-on-large-scale-deep-learning-at-google.html)
35. [7 Years Of YouTube Scalability Lessons In 30 Minutes](http://highscalability.com/blog/2012/3/26/7-years-of-youtube-scalability-lessons-in-30-minutes.html)
36. [6 Ways Not To Scale That Will Make You Hip, Popular And Loved By VCs](http://highscalability.com/blog/2011/4/18/6-ways-not-to-scale-that-will-make-you-hip-popular-and-loved.html)
37. [A Beginner's Guide To Scaling To 11 Million+ Users On Amazon's AWS](http://highscalability.com/blog/2016/1/11/a-beginners-guide-to-scaling-to-11-million-users-on-amazons.html)
38. [Building Super Scalable Systems: Blade Runner Meets Autonomic Computing in the Ambient Cloud](http://highscalability.com/blog/2009/12/16/building-super-scalable-systems-blade-runner-meets-autonomic.html)
39. [Peecho Architecture - Scalability On A Shoestring](http://highscalability.com/blog/2011/8/1/peecho-architecture-scalability-on-a-shoestring.html)
40. [Making The Case For Building Scalable Stateful Services In The Modern Era](http://highscalability.com/blog/2015/10/12/making-the-case-for-building-scalable-stateful-services-in-t.html)
41. [Scaling Pinterest - From 0 To 10s Of Billions Of Page Views A Month In Two Years](http://highscalability.com/blog/2013/4/15/scaling-pinterest-from-0-to-10s-of-billions-of-page-views-a.html)
42. [The Secret Of Scaling: You Can't Linearly Scale Effort With Capacity](http://highscalability.com/blog/2014/6/4/the-secret-of-scaling-you-cant-linearly-scale-effort-with-ca.html)
43. [Scalability As A Service](http://highscalability.com/blog/2014/12/22/scalability-as-a-service.html)
44. [How Shopify Scales To Handle Flash Sales From Kanye West And The Superbowl](http://highscalability.com/blog/2015/11/2/how-shopify-scales-to-handle-flash-sales-from-kanye-west-and.html)
45. [How Uber Scales Their Real-Time Market Platform](http://highscalability.com/blog/2015/9/14/how-uber-scales-their-real-time-market-platform.html)
46. [How Autodesk Implemented Scalable Eventing Over Mesos](http://highscalability.com/blog/2015/8/17/how-autodesk-implemented-scalable-eventing-over-mesos.html)
47. [Scaling Kim Kardashian To 100 Million Page Views](http://highscalability.com/blog/2015/2/16/scaling-kim-kardashian-to-100-million-page-views.html)
48. [How We Scale VividCortex's Backend Systems](http://highscalability.com/blog/2015/3/30/how-we-scale-vividcortexs-backend-systems.html)
49. [Latency is Everywhere and it Costs You Sales - How to Crush it](http://highscalability.com/blog/2009/7/25/latency-is-everywhere-and-it-costs-you-sales-how-to-crush-it.html)
50. [Useful Scalability Blogs](http://highscalability.com/blog/category/blog)[Pinterest Architecture Update - 18 Million Visitors, 10x Growth,12 Employees, 410 TB Of Data](http://highscalability.com/blog/2012/5/21/pinterest-architecture-update-18-million-visitors-10x-growth.html)
51. [Why Are Facebook, Digg, And Twitter So Hard To Scale?](http://highscalability.com/blog/2009/10/13/why-are-facebook-digg-and-twitter-so-hard-to-scale.html)
52. [Playfish's Social Gaming Architecture - 50 Million Monthly Users And Growing](http://highscalability.com/blog/2010/9/21/playfishs-social-gaming-architecture-50-million-monthly-user.html)
53. [Ask HighScalability: Facing Scaling Issues With News Feeds On Redis. Any Advice?](http://highscalability.com/blog/2012/8/13/ask-highscalability-facing-scaling-issues-with-news-feeds-on.html)
54. [Elements Of Scale: Composing And Scaling Data Platforms](http://highscalability.com/blog/2015/5/4/elements-of-scale-composing-and-scaling-data-platforms.html)
55. [Pursue Robust Indefinite Scalability With The Movable Feast Machine](http://highscalability.com/blog/2011/9/28/pursue-robust-indefinite-scalability-with-the-movable-feast.html)
56. [Tagged Architecture - Scaling To 100 Million Users, 1000 Servers, And 5 Billion Page Views](http://highscalability.com/blog/2011/8/8/tagged-architecture-scaling-to-100-million-users-1000-server.html)
57. [17 Techniques Used To Scale Turntable.Fm And Labmeeting To Millions Of Users](http://highscalability.com/blog/2011/9/26/17-techniques-used-to-scale-turntablefm-and-labmeeting-to-mi.html)
58. [5 Scalability Poisons And 3 Cloud Scalability Antidotes](http://highscalability.com/blog/2011/9/21/5-scalability-poisons-and-3-cloud-scalability-antidotes.html)
59. [The Secret Of Scaling: You Can't Linearly Scale Effort With Capacity](http://highscalability.com/blog/2014/6/4/the-secret-of-scaling-you-cant-linearly-scale-effort-with-ca.html)
60. [10 EBay Secrets For Planet Wide Scaling](http://highscalability.com/blog/2009/11/17/10-ebay-secrets-for-planet-wide-scaling.html)
61. [Scaling An AWS Infrastructure - Tools And Patterns](http://highscalability.com/blog/2010/8/16/scaling-an-aws-infrastructure-tools-and-patterns.html)
62. [Vertical Scaling Ascendant - How Are SSDs Changing Architectures?](http://highscalability.com/blog/2012/7/25/vertical-scaling-ascendant-how-are-ssds-changing-architectur.html)
63. [Strategy: Diagonal Scaling - Don't Forget To Scale Out AND Up](http://highscalability.com/blog/2007/11/5/strategy-diagonal-scaling-dont-forget-to-scale-out-and-up.html)
64. [Strategy: Three Techniques To Survive Traffic Surges By Quickly Scaling Your Site](http://highscalability.com/blog/2014/3/19/strategy-three-techniques-to-survive-traffic-surges-by-quick.html)
65. [Building Scalable Systems Using Data As A Composite Material](http://highscalability.com/blog/2009/11/16/building-scalable-systems-using-data-as-a-composite-material.html)
66. [Six Lessons Learned The Hard Way About Scaling A Million User System](http://highscalability.com/blog/2014/4/16/six-lessons-learned-the-hard-way-about-scaling-a-million-use.html)
67. [Sean Hull's 20 Biggest Bottlenecks That Reduce And Slow Down Scalability](http://highscalability.com/blog/2013/8/28/sean-hulls-20-biggest-bottlenecks-that-reduce-and-slow-down.html)
68. [Robert Scoble's Rules For Successfully Scaling Startups](http://highscalability.com/blog/2008/7/18/robert-scobles-rules-for-successfully-scaling-startups.html)
69. [The Four Meta Secrets Of Scaling At Facebook](http://highscalability.com/blog/2010/6/10/the-four-meta-secrets-of-scaling-at-facebook.html)
70. [The Changing Face Of Scale - The Downside Of Scaling In The Contextual Age](http://highscalability.com/blog/2013/3/27/the-changing-face-of-scale-the-downside-of-scaling-in-the-co.html)
71. [The 10 Deadly Sins Against Scalability](http://highscalability.com/blog/2013/6/10/the-10-deadly-sins-against-scalability.html)
72. [Scaling Mailbox - From 0 To One Million Users In 6 Weeks And 100 Million Messages Per Day](http://highscalability.com/blog/2013/6/18/scaling-mailbox-from-0-to-one-million-users-in-6-weeks-and-1.html)
73. [Evolution Of Bazaarvoice’s Architecture To 500M Unique Users Per Month](http://highscalability.com/blog/2013/12/2/evolution-of-bazaarvoices-architecture-to-500m-unique-users.html)
74. [How The AOL.Com Architecture Evolved To 99.999% Availability, 8 Million Visitors Per Day, And 200,000 Requests Per Second](http://highscalability.com/blog/2014/2/17/how-the-aolcom-architecture-evolved-to-99999-availability-8.html)
75. [Learn From My Pain - 5 Lessons From Ello's Adventures In Rapid Scaling](http://highscalability.com/blog/2015/1/21/learn-from-my-pain-5-lessons-from-ellos-adventures-in-rapid.html)
76. [HappyPancake: A Retrospective On Building A Simple And Scalable Foundation](http://highscalability.com/blog/2015/2/23/happypancake-a-retrospective-on-building-a-simple-and-scalab.html)
77. [Latency Exists, Cope!](https://web.archive.org/web/20181004043647/http://www.addsimplicity.com/adding_simplicity_an_engi/2007/02/latency_exists_.html) - Commentary on coping with latency and it's architectural impacts
78. [Latency - the new web performance bottleneck](https://www.igvita.com/2012/07/19/latency-the-new-web-performance-bottleneck/) - not at all new (see [Patterson](http://dl.acm.org/citation.cfm?id=1022596)), but noteworthy
79. [The Tail At Scale](https://research.google/pubs/pub40801/) - the latencychallenges inherent of dealing with latency in large scale systems
80. [Lessons Learned from Scaling Up Octopus • Mike Noonan • YOW! 2021](https://www.youtube.com/watch?v=Eie_kxwzyfQ) (video)
81. [От 0 до 200 000 000 игроков — об эволюции бэкенда за 40 мин / Андрей Михеев (Pixonic)](https://www.youtube.com/watch?v=QOUhunaFMOY) (video)
82. [Архитектура Warface: удержание нагрузки десятков тысяч подключений](https://www.youtube.com/watch?v=XxYwgeL71FE) (video)
83. [System Design — Scaling from Zero to Millions Of Users](https://medium.com/geekculture/system-design-scaling-from-zero-to-millions-of-users-deca270ef784)
84. [Как построить систему, способную выдерживать нагрузку в 5 млн rps](https://habr.com/ru/companies/ozontech/articles/749328/)
85. [Scaling for Success: The Load Balancing Journey of a Fictional Startup](https://zahere.com/scaling-for-success-the-load-balancing-journey-of-a-fictional-startup)

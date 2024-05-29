
**Transcoding** is the process of converting a video to different formats. It’s necessary because each client device supports a different format. While the **bit rate** represents the data in each second of the video.

# Top k requests during past time interval

1. [Efficient Computation of Frequent and Top-k Elements in Data Streams](http://www.cse.ust.hk/~raywong/comp5331/References/EfficientComputationOfFrequentAndTop-kElementsInDataStreams.pdf)
2. [An Optimal Strategy for Monitoring Top-k Queries in Streaming Windows](http://davis.wpi.edu/xmdv/docs/EDBT11-diyang.pdf)


# [**Stream Data Deduplication**](https://en.wikipedia.org/wiki/Data_deduplication)

1. [Exactly-once Semantics with Kafka](https://www.confluent.io/blog/exactly-once-semantics-are-possible-heres-how-apache-kafka-does-it/)
2. [Real-time Deduping at Tapjoy](http://eng.tapjoy.com/blog-list/real-time-deduping-at-scale)
3. [Deduplication at Segment](https://segment.com/blog/exactly-once-delivery/)
4. [Deduplication at Mail.Ru](https://medium.com/@andrewsumin/efficient-storage-how-we-went-down-from-50-pb-to-32-pb-99f9c61bf6b4)
5. [Petabyte Scale Data Deduplication at Mixpanel](https://medium.com/mixpaneleng/petabyte-scale-data-deduplication-mixpanel-engineering-e808c70c99f8)

# References:

1. **[How to Handle Real-Time Stream Processing](https://blog.bitsrc.io/real-time-stream-processing-f1c79a4dfe52)**
2. **[System Design for short video posts like TikTok, Insta Reel, Youtube Shorts](https://medium.com/nerd-for-tech/system-design-for-short-video-posts-like-tiktok-insta-reel-youtube-shorts-a873ad6b7d5b)**
3. [YouTube System Design](https://medium.com/geekculture/youtube-system-design-3d094f750135)
4. [System Design — Short Video App with a billion users](https://ashishguptabns.medium.com/system-design-short-video-app-with-a-billion-users-1b73c125d476)
5. [System Design: Video Streaming Platform](https://medium.com/@the.york.wei/system-design-video-streaming-platform-fa5e48cb0705)
6. [System Design : Amazon Prime Video](https://medium.com/@neo678/system-design-amazon-prime-video-25a2592377f5)
7. [System Design of Netflix](https://saxenasanket.medium.com/system-design-of-netflix-part-1-4d65642ed738)
8. [Netflix Design](https://medium.com/@karan99/system-design-netflix-6962b4f6222)
9. [How NOT to design Netflix in your 45-minute System Design Interview?](https://hackernoon.com/how-not-to-design-netflix-in-your-45-minute-system-design-interview-64953391a054)
10. [Netflix System Design | YouTube System Design | System Design Interview Question](https://www.youtube.com/watch?v=lYoSd2WCJTo) (video)
11. [NETFLIX System design | software architecture for netflix](https://www.youtube.com/watch?v=psQzyFfsUGU&list=PLkQkbY7JNJuBoTemzQfjym0sqbOHt5fnV&index=5) (video)
12. [System Design Netflix — A Complete Architecture](https://medium.com/@anuupadhyay1994/system-design-netflix-a-complete-architecture-d6d4db93757f)
13. [Design OTT Platform](https://shivam-sinha.medium.com/design-ott-platform-3727765c22a7)
14. [How Netflix onboards new content: Video Processing at scale](https://www.youtube.com/watch?v=x9Hrn0oNmJM&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=10) (video)
15. [Netflix: What Happens When You Press Play?](http://highscalability.com/blog/2017/12/11/netflix-what-happens-when-you-press-play.html)
16. [Scaling up the Prime Video audio/video monitoring service and reducing costs by 90%](https://www.primevideotech.com/video-streaming/scaling-up-the-prime-video-audio-video-monitoring-service-and-reducing-costs-by-90) Prime Video - a video streaming product of Amazon with their own technical blog - dropped a piece which exploded over all the technical communities I am participating. Twitter, work slacks, telegram groups - all are referencing this article. So what's the hype? Prime Video has a service called Video Quality Analysis. It is supposed to identify any problems with video streaming and report it for further fix and investigation. The initial architecture leveraged Amazon Lambdas and Step Functions, but most importantly it was distributed by nature, which caused the usage of an S3 bucket for data sharing between the microservices. Apparently, it is very costly! So after some consideration, the team decided to move to a monolith.
17. [Simone: Distributed Simulation Service at Netflix](https://medium.com/netflix-techblog/https-medium-com-netflix-techblog-simone-a-distributed-simulation-service-b2c85131ca1b)
18. [Media Database at Netflix](https://medium.com/netflix-techblog/implementing-the-netflix-media-database-53b5a840b42a)
19. [How Netflix onboards new content onto their platform](https://medium.com/@interviewready/how-netflix-onboards-new-content-onto-their-platform-1350528325d9)
20. [How Facebook Live Streams To 800,000 Simultaneous Viewers](http://highscalability.com/blog/2016/6/27/how-facebook-live-streams-to-800000-simultaneous-viewers.html)
21. [Scaling Live Videos to a Billion Users at Facebook - Sachin Kulkarni, Director of Engineering at Facebook](https://www.youtube.com/watch?v=IO4teCbHvZw) (video)
22. [Open sourcing Brooklin: Near real-time data streaming at scale](https://engineering.linkedin.com/blog/2019/brooklin-open-source)
23. [Keystone Real-time Stream Processing Platform](https://netflixtechblog.com/keystone-real-time-stream-processing-platform-a3ee651812a)
24. [Transcoding Videos at Scale](https://www.egnyte.com/blog/2018/12/transcoding-how-we-serve-videos-at-scale/)
25. [Facebook Video Broadcasting](https://engineering.fb.com/ios/under-the-hood-broadcasting-live-video-to-millions/)
26. [Netflix Video Encoding at Scale](https://netflixtechblog.com/high-quality-video-encoding-at-scale-d159db052746)
27. [Netflix Shot based encoding](https://netflixtechblog.com/optimized-shot-based-encodes-now-streaming-4b9464204830)
28. [MapReduce: Simplified Data Processing on Large Clusters](https://research.google/pubs/pub62/)
29. [System Design Interview - Top K Problem](https://www.youtube.com/watch?v=kx-XDoPjoHw) (video)
30. [Платформа 4К-стриминга на миллион онлайнов](https://www.youtube.com/watch?v=pqz_qld6jVA&list=PLH-XmS0lSi_wRIh4RJjnTGMKaTiQoaGTc&index=59) (video)
31. [Почти без магии, или Как просто раздать терабит видеопотока](https://www.youtube.com/watch?v=bA-Y_BfTZZY&list=PLH-XmS0lSi_wRIh4RJjnTGMKaTiQoaGTc&index=61) (video)
32. [Justin.Tv's Live Video Broadcasting Architecture](http://highscalability.com/blog/2010/3/16/justintvs-live-video-broadcasting-architecture.html)
33. [Cinchcast Architecture - Producing 1,500 Hours Of Audio Every Day](http://highscalability.com/blog/2012/7/16/cinchcast-architecture-producing-1500-hours-of-audio-every-d.html)
34. [VK Видео: архитектура крупнейшей видеоплатформы на 4 Тбит/с / Александр Тоболь (ВКонтакте)](https://www.youtube.com/watch?v=8ICxQ-UPVn0) (video)
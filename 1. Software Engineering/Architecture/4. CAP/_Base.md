_In a distributed computer system, you can only support two of the following guarantees:_

- _**[[Consistency]]** - Every read receives the most recent write or an error_
- _**[[Availability]]** - Every request receives a response, without guarantee that it contains the most recent version of the information_
- _**[[Partition Tolerance]]** - The system continues to operate despite arbitrary partitioning due to network failures_

_Networks aren't reliable, so you'll need to support partition tolerance. You'll need to make a software tradeoff between consistency and availability._

### _CP - consistency and partition tolerance_

_Waiting for a response from the partitioned node might result in a timeout error. CP is a good choice if your business needs require atomic reads and writes._

### _AP - availability and partition tolerance_

_Responses return the most readily available version of the data available on any node, which might not be the latest. Writes might take some time to propagate when the partition is resolved._

_AP is a good choice if the business needs to allow for [eventual consistency](https://github.com/donnemartin/system-design-primer#eventual-consistency) or when the system needs to continue working despite external errors._

![[Pasted image 20231226132747.png]]
![[Pasted image 20231226132759.png]]

Теорема CAP утверждает, что распределенная система не может одновременно обеспечивать более двух из этих трех гарантий:  **(C)onsistency** – согласованность, во всех серверах в один момент времени данные не противоречат друг другу. **(A)vailability** - доступность, любой запрос к системе завершается корректным откликом, однако, без гарантии, что ответы всех серверов системы совпадают. **(P)artition Tolerance** - устойчивость к разделению, разделение системы на несколько изолированных секций не приводит к некорректности отклика от каждой из секций.

Грани треугольника: **PA** – в случае сетевого разделения узлов системы она продолжает отвечать на запросы, при этом не гарантируя согласованности данных. Примеры: Cassandra, CouchDB. **PC** – в случае сетевого разделения узлов системы она прекращает отвечать на запросы, данные остаются согласованными. Примеры: MongoDB, Redis. **CA** – в случае отсутствия сетевого разделения данные доступны и согласованы. Примеры: MySQL, Postgres.

🆘 Latency(время ответа на запрос) Если в кластере из 10 узлов «погибает» 9, то оставшийся мастер не сможет обслужить всех пользователей с удовлетворительным latency.

✅ **PACELC-теорема**, расширение CAP-теоремы. IF P THEN (A or C) ELSE (L or C) В случае сетевого разделения (P) в распределенной системе необходимо выбирать между доступностью (A) и согласованностью (C), как и в CAP- теореме, но в остальном (E, ELSE), даже при нормальной работе системы без разделения, нужно выбирать между задержкой (L) и согласованностью (C).

🛠️ Грани ромба: **PA/EL** – высокая доступность (A) при разделении (P) системы, иначе (E) высокая скорость ответа (L). Примеры: Cassandra. **PC/EC** – согласованность (С) при разделении (P) системы, иначе (E) согласованность (С). Примеры: MySQL, Postgres. **PC/EL** – согласованность (С) при разделении (P) системы, иначе (E) высокая скорость ответа (L). Примеры: PNUTS. **PA/EC** – высокая доступность (A) при разделении (P) системы, иначе (E) согласованность (С). Примеры: MongoDB.


# 1. [[Consistency]]

# 2. [[Availability]]

# 3. [[Partition tolerance]]


# How To:
## 1. If your system goes slow ⇒ Scalability (Partition tolerance)

> _Understand your problems: scalability problem (fast for a single user but slow under heavy load) or performance problem (slow for a single user) by reviewing some [design principles](https://github.com/binhnguyennus/awesome-scalability#principle) and checking how [scalability](https://github.com/binhnguyennus/awesome-scalability#scalability) and [performance](https://github.com/binhnguyennus/awesome-scalability#performance) problems are solved at tech companies. The section of [intelligence](https://github.com/binhnguyennus/awesome-scalability#:~:text=The%20section%20of-,intelligence,-are%20created%20for) are created for those who work with data and machine learning at big (data) and deep (learning) scale._

### 2. If your system goes down ⇒ Availability

> _"Even if you lose all one day, you can build all over again if you retain your calm!" - Thuan Pham, former CTO of Uber. So, keep calm and mind the [availability](https://github.com/binhnguyennus/awesome-scalability#availability) and [stability](https://github.com/binhnguyennus/awesome-scalability#stability) matters!_

### 3. If you are having a system design interview

> _Look at some interview notes and real-world architectures with completed diagrams to get a comprehensive view before designing your system on whiteboard. You can check some talks of engineers from tech giants to know how they build, scale, and optimize their systems. There are some selected books for you (most of them are free)! Good luck!_


# References:

1. [CAP Theorem Simplified](https://vishalrana9915.medium.com/cap-theorem-simplified-de3ddcc1f09e)
2. [CAP theorem revisited](http://robertgreiner.com/2014/08/cap-theorem-revisited/)
3. [A plain english introduction to CAP theorem](http://ksat.me/a-plain-english-introduction-to-cap-theorem)
4. [CAP Theorem — An impossible choice](https://medium.com/@toxicdev/cap-theorem-an-impossible-choice-c04482b8a36c)
5. [The CAP theorem](https://www.youtube.com/watch?v=k-Yaq8AHlFA)
6. [CAP Theorem and Trade-offs](http://robertgreiner.com/2014/08/cap-theorem-revisited/)
7. [System design fundamentals: What is the CAP theorem?](https://www.educative.io/blog/what-is-cap-theorem#whatiscaptheorem)
8. [An Illustrated Proof of the CAP Theorem](https://mwhittaker.github.io/blog/an_illustrated_proof_of_the_cap_theorem/)
9. [The CAP FAQ](https://www.the-paper-trail.org/page/cap-faq/)
10. [My thoughts on the CAP theorem](https://www.youtube.com/watch?v=KmGy3sU6Xw8&list=PLQnljOFTspQXNP6mQchJVP3S-3oKGEuw9&index=73) (video)
11. [](https://github.com/donnemartin/system-design-primer#cap-theorem)[https://github.com/donnemartin/system-design-primer#cap-theorem*](https://github.com/donnemartin/system-design-primer#cap-theorem*)
12. [Spanner, TrueTime & The CAP Theorem](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/45855.pdf)
13. [Harvest, Yield and Scalable Tolerant Systems](https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.33.411) - Real world applications of CAP from Brewer et al*
14. [CAP Conjecture](https://web.archive.org/web/20190629112250/https://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) - Consistency, Availability, Parition Tolerance cannot all be satisfied at once*
15. [CAP Twelve Years Later: How the "Rules" Have Changed](https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed) - Eric Brewer expands on the original tradeoff description*
16. [Забудьте САР теорему как более не актуальную](https://habr.com/ru/articles/258145/)
17. [CAP vs PACELC](https://www.designgurus.io/blog/system-design-interview-basics-cap-vs-pacelc) [CAP and Consistenc](https://www.yugabyte.com/blog/a-for-apple-b-for-ball-c-for-cap-theorem/)
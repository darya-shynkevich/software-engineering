
1) Architect products; evolve from projects to product
2) Focus on quality attributes, not on functional requirements
3) Delay design decisions until they are absolutely necessary
4) Architect for change - leverage the "power of small"
5) Architect for build, test, deploy, and operate
6) Model the organization of your teams after the design of the system you are working on

Distributed Systems:
	1. Multiple agents
	2. Global properties
	3. Localises information
	4. Partial Failure 

*[Fallacies of Distributed Systems](../Fallacies%20of%20Distributed%20Systems.md) are a set of assertions made by L Peter Deutsch and others at Sun Microsystems describing false assumptions that programmers new to distributed applications invariably make.*

*[Microservices](../5.%20Architecture%20Types/Monolith%20vs.%20Microservices/Microservices.md) - also known as the microservice architecture - is an architectural style that structures an application as a collection of services that are*

- *Highly maintainable and testable
- *Loosely coupled*
- *Independently deployable*
- *Organized around business capabilities*
- *Owned by a small team*

*The microservice architecture enables the rapid, frequent and reliable delivery of large, complex applications. It also enables an organization to evolve its technology stack.*

1. [DNS](../../7.%20Networks/5.%20DNS.md) - translates human-friendly domain names, like “[www.example.com](http://www.example.com/)," into machine-readable IP addresses, such as “192.168.1.1.”
2. [Load Balancers](../../7.%20Networks/LB/_Base.md) -  is a network device or software application that is used to evenly distribute incoming network traffic among multiple servers.
3. [API Gateway](../1.%20Concepts/API%20Gateway.md) -  an intermediary between external clients and the backend services or microservices within an application
4. [CDN](../../7.%20Networks/4.%20CDN.md) - caches content close to end users
5. [Proxy](../../7.%20Networks/Proxy/_Base.md) - functions as an intermediary between client computers and the internet
6. [Cache](../2.%20Components/Cache/0.%20Cache.md) - is a high-speed storage layer situated between an application and an original data source, such as a database, file system, or remote network service.
7. [Partitioning Strategies](../../3.%20Database/OTLP/SQL/5.%20Distributed/Partitioning/Partitioning%20Strategies.md) - divides data within tables into smaller units called partitions.
8. [Replication](../../3.%20Database/OTLP/SQL/5.%20Distributed/Replication/Base.md) - is a strategy employed to maintain multiple copies of a single database across different servers or locations, with the aim of enhancing data availability, redundancy, and fault tolerance.
9. [Distributed Messaging System](../2.%20Components/Brokers/Distributed%20Messaging%20System.md) - serves as a vital component that facilitates communication among diverse applications, services, or components located across various geographical locations.
10. [Microservices](../5.%20Architecture%20Types/Monolith%20vs.%20Microservices/Microservices.md) - a way of building software applications by breaking them into small, separate parts that can work together.
11. [NoSQL](../../3.%20Database/OTLP/NoSQL/Base.md) - is a non-relational database system designed to efficiently handle the storage and retrieval of unstructured or semi-structured data.
12. [Indexes](../../3.%20Database/OTLP/SQL/2.%20Indexes/_Base.md) - a specialized structure designed to accelerate the speed and efficiency of query operations within a database.
13. [Distributed File System (DFS)](../2.%20Components/Distributed%20File%20System%20(DFS).md) - is a storage solution strategically designed to oversee and facilitate access to files and directories across numerous servers, nodes, or machines, which are often spread out across a network.
14. [Full-Text Search](../../3.%20Database/OTLP/SQL/2.%20Indexes/Full-Text%20Search.md) - a powerful tool that helps users search for specific words or phrases on a website or app.

# Books:

1. [Distributed systems for fun and profit](https://book.mixu.net/distsys/single-page.html?utm_source=substack&utm_medium=email)
2. [Web Scalability for Startup Engineers](https://twitter.com/_abstractart/status/1642591282403868673?t=3C9l0QpoCb0cJHw0vC85nA&s=35) (book)
3. [Highload (курс от ФПМИ)](https://www.youtube.com/watch?v=7g01DlHlQqI&list=PL4_hYwCyhAvYyx4TIRk6tLG0c8CLGzhE5) (videos, course)
4. [Designing Data-Intensive Applications: The Big Ideas Behind Reliable, Scalable, and Maintainable Systems](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321/) (book) + [Tinkoff Book Club](https://www.youtube.com/playlist?list=PLLrf_044z4JrBw4S8bCgf77kPO8xrxczU)
5. [Software Architecture: The Hard Parts: Modern Trade-Off Analyses for Distributed Architectures](http://libgen.rs/book/index.php?md5=6ABDBA667E209C75F49D97E44BC16B5C) (book) + [Tinkoff Book Club](https://www.youtube.com/playlist?list=PLLrf_044z4Jp1zFQk-487YCK20JPSE3l8) + [Recap](https://apolomodov.medium.com/code-of-architecture-recap-of-software-architecture-the-hard-parts-a2d31be999f3)
6. [Software Architecture Patterns](https://www.oreilly.com/library/view/software-architecture-patterns/9781098134280/) (book)
7. [Fundamentals of Software Architecture: A Comprehensive Guide to Patterns, Characteristics, and Best Practices](http://library.lol/main/1FC05CFFA6DD29E55EC4D4980BFF0BB1) (book) + [O’Reilly Book Club](https://www.oreilly.com/library/view/oreilly-book-club/0636920876540/) (video) + [Superstream](https://www.oreilly.com/library/view/software-architecture-superstream/0636920557999/) (video)
8. [Microservices vs. Service-Oriented Architecture](https://learning.oreilly.com/library/view/microservices-vs-service-oriented/9781491975657/) (book) + [video](https://www.oreilly.com/library/view/service-based-architectures/9781491932636/)
9. [Microservices AntiPatterns and Pitfalls](https://learning.oreilly.com/library/view/microservices-antipatterns-and/9781492042716/) (book) + [video](https://www.oreilly.com/library/view/microservices-antipatterns-and/9781491963937/)
10. [Fundamentals of Software Architecture: An Engineering Approach](http://libgen.rs/book/index.php?md5=31649FBCB4205FCFF176587310474F65) (book)
11. [TFTDS (курс от ФПМИ)](https://www.youtube.com/watch?v=eRmhfw7hqdw&list=PL4_hYwCyhAvZd6B5fN3yAB0zOCjhgpfgg) (course, videos)
12. Эндрю Таненбаум — [Распределенные системы. Принципы и парадигмы](http://libgen.rs/book/index.php?md5=0C79DF856F16453256F892ACC218F66F) (book) + где-то было обсуждение глав
13. [Distributed Systems lecture series](https://www.youtube.com/playlist?list=PLeKd45zvjcDFUEv_ohr_HdUFe97RItdiB) (course, video)
14. [Distributed Systems: Principles and Paradigms](https://www.distributed-systems.net/index.php/books/ds4/) (book) + [Обсуждение](https://apolomodov.medium.com/code-of-architecture-recap-of-distributed-systems-4th-edition-89cc10b282c) + [Tinkoff Book Club](https://www.youtube.com/playlist?list=PLLrf_044z4Jr299tuUm1G8bfJrBF_eEsx)
15. [Systems Design Fundamentals](https://www.algoexpert.io/systems/fundamentals) (course, videos)
16. [Patterns of Enterprise Application Architecture](https://www.amazon.com/Patterns-Enterprise-Application-Architecture-Martin/dp/0321127420) (book)
17. [Harvard lecture on scalability.](https://www.youtube.com/watch?v=-W9F__D3oY4) (video)
18. [Инфраструктура больших данных (курс от ФПМИ)](https://www.youtube.com/watch?v=UoJ6qy1xwG4&list=PL4_hYwCyhAvbI83zAjpg8EO52sBywa2rQ&index=1) (course, videos)
19. [Continuous Architecture in Practice: Software Architecture in the Age of Agility and DevOps (Addison-Wesley Signature Series (Vernon))](http://libgen.rs/book/index.php?md5=CACA2FD49821751D57CCAAE39ACCAD5C) (book) + More materials
20. 20. Если есть время - попробую пройти курс от MIT по распределенным системам. Там есть лабораторки на Golang. Как по мне - гораздо продуктивно изучать концепции map-reduce, распределенных kv, raft на практике [https://pdos.csail.mit.edu/6.824/schedule.html](https://pdos.csail.mit.edu/6.824/schedule.html)
21. [The Architecture of Open Source Applications](https://aosabook.org/en/index.html) (book)
22. [Building Evolutionary Architectures](http://libgen.rs/book/index.php?md5=FE95410D87E1CDE47BEAA790026137DB) (book) + [Tinkoff Book Club](https://www.youtube.com/playlist?list=PLLrf_044z4JqFwcckHOIubViLXBSsmA1A) + [GOTO conference](https://www.youtube.com/watch?v=m2ZlX1je3as) + More Materials
23. [A Philosophy of Software Design](http://libgen.rs/book/index.php?md5=D82934E270545B59B64E843B2930FE57) (book) + [Tinkoff Book Club](https://www.youtube.com/playlist?list=PLLrf_044z4Jp0CZ2o2s-rJDDxZgnNWqJX)
24. [Technology Strategy Patterns: Architecture as Strategy](http://library.lol/main/B8D927E305B792C39BA1DCB769C1A227) (book) + [Tinkoff Book Club](https://www.youtube.com/playlist?list=PLLrf_044z4Jq5PwfgLpzaL-Ymcocir_X3)

# References:

1. ! [Букварь по дизайну систем (Ольховой Д.Р.)](https://docs.google.com/document/d/1w3qb6SS1Hycyce5Fg5mVMdzkGYXTRskSf57IoD98ZQw/edit#heading=h.7sot09tq18su)
2. [System design primer: Learn the basics of system design](https://www.educative.io/blog/system-design-primer)
3. ! [**Leader Election Algorithms in Distributed Systems**](https://medium.com/nerd-for-tech/leader-election-algorithms-in-distributed-systems-f513d41ad0d9)
4. [What choose from Vertical/Horizontal/cache in System Design](https://medium.com/@komalpal/what-choose-from-vertical-horizontal-cache-in-system-design-1f5e5e5b183c)
5. **[System Design Cheatsheet: Databases](https://levelup.gitconnected.com/system-design-cheatsheet-databases-43ec82de2260)**
6. **[HighLoad++ для начинающих](https://highload.guide/blog/highload-for-beginners.html)** (лекции)
7. **[Анатомия веб-сервиса](https://highload.guide/blog/inside-webserver.html)** (лекция)
8. [Архитектура IT решений](https://www.youtube.com/watch?v=Lq17AMCMLAE) (video)
9. [Architecture Pattern: Orchestration via Workflows](https://kislayverma.com/software-architecture/architecture-pattern-orchestration-via-workflows/)
10. [Lessons from Giant-Scale Services - Eric Brewer, UC Berkeley & Google](https://people.eecs.berkeley.edu/~brewer/papers/GiantScale-IEEE.pdf)
11. [System Design Interview – Step By Step Guide](https://www.youtube.com/watch?v=bUHFg8CZFws) (video)
12. [Distributed Systems in One Lesson - Tim Berglund, Senior Director of Developer Experience at Confluent](https://www.youtube.com/watch?v=Y6Ev8GIlbxc) (video)
13. [System Design Interview Basics](https://www.youtube.com/playlist?list=PLOAph0xkZvSvCX3Pk3S68WY14BKYN_w64) (videos)
14. [System Design Interview - Tips & Tricks | Biggest Mistakes to Avoid](https://www.youtube.com/watch?v=4Q2fokImKfM) (video)
15. [Database Design Tips | Choosing the Best Database in a System Design Interview](https://www.youtube.com/watch?v=cODCpXtPHbQ) (video)
16. [Learn System design : Distributed Systems Introduction | Horizontal scaling vertical scaling](https://www.youtube.com/watch?v=OyTEd9h_CVQ&list=PLkQkbY7JNJuBoTemzQfjym0sqbOHt5fnV&index=13) (video)
17. [Learn System design : Distributed datastores | RDBMS scaling problems | CAP theorem](https://www.youtube.com/watch?v=l9JSK9OBzA4&list=PLkQkbY7JNJuBoTemzQfjym0sqbOHt5fnV&index=14) (video)
18. [Learn System design : How distributed datastore works(basics)?](https://www.youtube.com/watch?v=ZbyYvTfBlE0&list=PLkQkbY7JNJuBoTemzQfjym0sqbOHt5fnV&index=15)
19. [Watch this before your System design interview!!](https://www.youtube.com/watch?v=pWO07iEpjO4&list=PLkQkbY7JNJuBoTemzQfjym0sqbOHt5fnV&index=20)
20. [When Designing a Backend System Minimize the “What If” Questions](https://www.youtube.com/watch?v=1a7E0qh48gM&list=PLQnljOFTspQXSevtRqvMNycWfHM7cXc3d&index=4) (video)
21. [Application layer](https://github.com/donnemartin/system-design-primer#application-layer) Separating out the web layer from the application layer (also known as platform layer) allows you to scale and configure both layers independently. Adding a new API results in adding application servers without necessarily adding additional web servers. The **single responsibility principle** advocates for small and autonomous services that work together. Small teams with small services can plan more aggressively for rapid growth.

	#### **Thought Provokers:**
	
	1. [On Designing and Deploying Internet Scale Services](https://mvdirona.com/jrh/talksAndPapers/JamesRH_Lisa.pdf) - James Hamilton
	2. [The Perils of Good Abstractions](https://web.archive.org/web/20181006111158/http://www.addsimplicity.com/adding_simplicity_an_engi/2006/12/the_perils_of_g.html) - Building the perfect API/interface is difficult
	3. [Chaotic Perspectives](https://web.archive.org/web/20180821164750/http://www.addsimplicity.com/adding_simplicity_an_engi/2007/05/chaotic_perspec.html) - Large scale systems are everything developers dislike - unpredictable, unordered and parallel
	4. [Data on the Outside versus Data on the Inside](http://cidrdb.org/cidr2005/papers/P12.pdf) - Pat Helland
	5. [Memories, Guesses and Apologies](https://channel9.msdn.com/Shows/ARCast.TV/ARCastTV-Pat-Helland-on-Memories-Guesses-and-Apologies) - Pat Helland
	6. [SOA and Newton's Universe](https://web.archive.org/web/20190719121913/https://blogs.msdn.microsoft.com/pathelland/2007/05/20/soa-and-newtons-universe/) - Pat Helland
	7. [Building on Quicksand](https://arxiv.org/abs/0909.1788) - Pat Helland
	8. [Stevey's Google Platforms Rant](https://web.archive.org/web/20190319154842/https://plus.google.com/112678702228711889851/posts/eVeouesvaVX) - Yegge's SOA platform experience
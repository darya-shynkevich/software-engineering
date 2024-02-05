1. Client-Server 
	1. клиенты не связаны с хранением данных
	2. сервер не связан с отображением данных
	3. клиенты отделены от сервера единым интерфейсом
2. Stateless (в запросе вся необходимая инфо: спросил - ответил - забыл)
3. Разделение на слои (клиент не должен знать работает ли он с конечной точкой или цепочкой сервисов)
4. Cache (ответ сервера может быть помечен как кэшируемый на определенный промежуток времени и может использоваться повторно без новых запросов к серверу)
5. Универсальный интерфейс
	1. ссылка как `id` ресурса
	2. манипуляция ресурсами через API, не БД
	3. самоописывающиеся сообщения
	4. гипермедиа
6. Код по требованию (по запросу сервер должен отдавать исполняемый код в виде апплета / сценария
7. )

REST - набор принципов и ограничений взаимодействия клиенты и сервера в сети интернет, использующий существующие стандарты HTTP, JSON/XML, ... в ходе взаимодействия

# References:

1. [Lesson 137 - Rest vs. Messaging](https://www.youtube.com/watch?v=gzQc-jvE22A)
2. 1. Roy Thomas Fielding: “[Architectural Styles and the Design of Network-Based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/fielding_dissertation.pdf),” PhD Thesis, University of California, Irvine, 2000.
2. Roy Thomas Fielding: “[REST APIs Must Be Hypertext-Driven](http://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven),” [roy.gbiv.com](http://roy.gbiv.com), October 20 2008.
3. “[REST in Peace, SOAP](https://royal.pingdom.com/rest-in-peace-soap/),” [royal.pingdom.com](http://royal.pingdom.com), October 15, 2010.
4. Stefan Tilkov: “[Interview: Pete Lacey Criticizes Web Services](http://www.infoq.com/articles/pete-lacey-ws-criticism),” [infoq.com](http://infoq.com), December 12, 2006.
5. [Representational state transfer (REST)](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest)
6. [Understanding State Transfer in REST (Explained by Example)](https://www.youtube.com/watch?v=EKCM1oQQrCM&list=PLQnljOFTspQV1emqxKbcP5esAf4zpqWpe&index=5) (video)
7. [Why Idempotency is very critical in Backend Applications](https://www.youtube.com/watch?v=4OuaONkZw1I&list=PLQnljOFTspQV1emqxKbcP5esAf4zpqWpe&index=24) (video)
8. [Стажёр Вася и его истории об идемпотентности API](https://m.habr.com/ru/company/yandex/blog/442762/)
9. [Stateful vs Stateless Applications (Explained by Example)](https://www.youtube.com/watch?v=nFPzI_Qg3FU&list=PLQnljOFTspQV1emqxKbcP5esAf4zpqWpe&index=8) (video)
10. [Examples of Stateful vs Stateless web applications with Python](https://www.youtube.com/watch?v=nhwZn6v5vT0&list=PLQnljOFTspQV1emqxKbcP5esAf4zpqWpe&index=3) (video)
11. [Amazon Alexa is a Stateless Application, Here is Why](https://www.youtube.com/watch?v=zhwMv5RxGew&list=PLQnljOFTspQV1emqxKbcP5esAf4zpqWpe&index=10) (video)
12. [When to Build a Stateless vs Stateful Back-ends using the right protocols (UDP, HTTP, TCP, QUIC)](https://www.youtube.com/watch?v=3E7HF26XNfU&list=PLQnljOFTspQWGuRmwojJ6LiV0ejm6eOcs&index=28) (video)
13. [How to GET a Cup of Coffee](https://www.infoq.com/articles/webber-rest-workflow/)
14. [How to Design a Good API & Why it Matters - Joshua Bloch, CMU & Google](https://www.infoq.com/presentations/effective-api-design) (video)
15. [Principles & Best practices of REST API Design](https://www.linkedin.com/pulse/principles-best-practices-rest-api-design-omar-ismail/)
16. [What RESTful actually means](https://codewords.recurse.com/issues/five/what-restful-actually-means)
17. [RESTful API Design: 13 Best Practices to Make Your Users Happy](https://florimond.dev/en/posts/2018/08/restful-api-design-13-best-practices-to-make-your-users-happy/)
18. [Безопасность REST API от А до ПИ](https://m.habr.com/ru/post/503284/)
19. [Welcome to the REST CookBook](https://restcookbook.com/) (book)
20. [REST worst practices](https://jacobian.org/2008/nov/14/rest-worst-practices/)
21. [API Best Practices: Webhooks, Deprecation, and Design](https://zapier.com/engineering/api-best-practices/)
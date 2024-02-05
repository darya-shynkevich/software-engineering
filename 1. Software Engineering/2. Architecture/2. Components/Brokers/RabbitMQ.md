RabbitMQ serves as a widely adopted message broker that employs the Advanced Message Queuing Protocol (AMQP). It supports various messaging patterns, encompassing point-to-point and publish-subscribe models. Messages are directed to exchanges and subsequently routed to queues based on routing keys. Consumers can then retrieve messages from these queues. RabbitMQ encompasses features like message acknowledgment, priority queues, and adaptable routing, positioning it well for scenarios demanding task distribution, event-driven architectures, and inter-service communication.

1. Pub-Sub
2. Point-to-Point

Protocols:
1. AMQP
2. MQTT
3. STOMP

Delivery:
1. At most once
2. At least once

В упрощённом виде принципы работы RabbitMQ можно представить так: приложение-отправитель (Publisher) публикует сообщения в брокер, ссылаясь на его внутреннюю сущность Exchange (Обменник). Обменник в зависимости от типа и настроек перенаправляет сообщения в одну или более связанных с ним очередей (Queue). Приложения-подписчики (Consumer) держат постоянное TCP соединение с RabbitMQ и ждут сообщения из заданной очереди. Брокер отправляет (push), распределяет сообщения между подписчиками. Если у очереди несколько подписчиков, сообщения между ними распределяются равномерно. Если сообщение успешно обработано подписчиком, оно удаляется из очереди.

![](../../../../../_Attachments/Pasted%20image%2020240125121133.png)

# References:

1. [RabbitMQ Crash Course](https://www.youtube.com/watch?v=Cie5v59mrTg&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=3) (video)
2. [Should RabbitMQ Implement QUIC Protocol for their Channels Message Queue?](https://www.youtube.com/watch?v=4Z3MAsdrEi8&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=1) (video)
3. [~~Publish-Subscribe Architecture (Explained by Example)](https://www.youtube.com/watch?v=O1PgqUqZKTA&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=4) (video)~~
4. [What is a Message Queue and When should you use Messaging Queue Systems Like RabbitMQ and Kafka](https://www.youtube.com/watch?v=W4_aGb_MOls&list=PLQnljOFTspQVcumYRWE2w9kVxxIXy_AMo&index=10) (video)
5. [RabbitMQ: терминология и базовые сущности](https://habr.com/ru/companies/southbridge/articles/703060/)
6. [Dabbling around Rabbit MQ persistence, durability & message routing](https://kousiknath.medium.com/dabbling-around-rabbit-mq-persistence-durability-message-routing-f4efc696098c)
7. [Типовое использование RabbitMQ](https://habr.com/ru/companies/southbridge/articles/714358/)
8. [Distributed systems with RabbitMQ](https://apirobot.me/posts/distributed-systems-with-rabbitmq)
9. [System Design Interview - Distributed Message Queue](https://www.youtube.com/watch?v=iJLL-KPqBpM) (video)
10. [Система отложенного исполнения на RabbitMQ](https://habr.com/ru/company/domclick/blog/519044/)
11. [Scheduling Messages with RabbitMQ](https://blog.rabbitmq.com/posts/2015/04/scheduling-messages-with-rabbitmq)
12. [Сергей Васечко, Точка. Менеджер распределённых заданий на кролике без celery](https://www.youtube.com/watch?app=desktop&v=jrzxAsHFvtI)
13. [Как запускать RabbitMQ в Docker](https://habr.com/ru/companies/southbridge/articles/704208/)
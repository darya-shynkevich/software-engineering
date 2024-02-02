
Принцип «Умный брокер, тупой потребитель» по отношению к [RabbitMQ](RabbitMQ.md) означает, что брокер берёт на себя много дополнительных действий. Например, следит за прочитанными сообщениями и удаляет их из очереди. Или сам организует процесс распределения сообщений между подписчиками

Для [[Kafka]] принцип «Тупой брокер, умный потребитель» означает, что, в отличие от RabbitMQ, он не занимается контролем и распределением сообщений. Потребители сами опрашивают брокер и решают, какие сообщения им читать, брокер только хранит данные.

# References:

1. ~~[Чем различаются Kafka и RabbitMQ: простыми словами](https://habr.com/ru/companies/innotech/articles/698838/)~~
2. [What’s the Difference Between Kafka and RabbitMQ?](https://aws.amazon.com/compare/the-difference-between-rabbitmq-and-kafka/#:~:text=RabbitMQ%20is%20a%20general%2Dpurpose,why%20they%20handle%20messaging%20differently)

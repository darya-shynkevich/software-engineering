Transaction concurrency problems can be solved by [[Transaction Loks]].

BUT we forget about one very important thing in the database — performance.

# [[Transaction Loks]]

# Версионирование данных [[MVCC]]

1. контроль целостности данных без блокировок
2. нет ожидания когда вовлеченные в конфликт транзакции отменяются и перезапускаются
3. каждая транзакция работает со своей версией данных
4. улучшение производительности
	1. читающий не блокирует пишущего и наоборот
	2. диспетчер блокировок не оказывает влияние 
5. Временная метка - уникальный ID, создаваемый БД для обозначения относительного момента времени запуска транзакции:
	1. системные часы
	2. счетчик

[[MVCC]] is a lock-free solution to track multiple versions (snapshots) of records to achieve high performance and concurrency safety.

















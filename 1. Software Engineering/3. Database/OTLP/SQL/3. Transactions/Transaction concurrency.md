Transaction concurrency problems can be solved by [_Base](Transaction%20Locks/_Base.md).

BUT we forget about one very important thing in the database — performance.

# [Transaction Locks](Transaction%20Locks/_Base.md)

# Версионирование данных [MVCC](MVCC.md)

1. контроль целостности данных без блокировок
2. нет ожидания когда вовлеченные в конфликт транзакции отменяются и перезапускаются
3. каждая транзакция работает со своей версией данных
4. улучшение производительности
	1. читающий не блокирует пишущего и наоборот
	2. диспетчер блокировок не оказывает влияние 
5. Временная метка - уникальный ID, создаваемый БД для обозначения относительного момента времени запуска транзакции:
	1. системные часы
	2. счетчик

[MVCC](MVCC.md) is a lock-free solution to track multiple versions (snapshots) of records to achieve high performance and concurrency safety.

















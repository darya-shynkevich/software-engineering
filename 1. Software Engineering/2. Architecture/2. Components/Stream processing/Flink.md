Apache Flink is a framework and distributed processing engine for stateful computations over unbounded and bounded data streams. Flink has been designed to run in all common cluster environments, perform computations at in-memory speed, and at any scale.

Alternatives:
1. Kafka Streams
2. Apache Spark 
3. Tez
4. Storm

Flink is a standalone stream processing engine that is deployed independently. 

Flink runs your application in a Flink cluster that you somehow deploy. 
Flink provides its own solutions to the hard problems faced by a distributed stream processing system, such as 
1. fault tolerance
2. exactly once delivery
3. high throughput
4. low latency

Those solutions involve 
1. checkpoints, 
2. savepoints, 
3. state management, 
4. time semantics.

# References:

1. ~~[What is Apache Flink?](https://www.confluent.io/learn/apache-flink/)~~
2. ~~[Apache Flink - A Must-Have For Your Streams | Systems Design Interview 0 to 1 With Ex-Google SWE](https://www.youtube.com/watch?v=fYO5-6Owt0w) (video)~~
Comes with a code generation tool that takes a schema definition like the ones shown here, and produces classes that implement the schema in various programming languages. Your application code can call this generated code to encode or decode records of the schema.

```Protobuf
 message Person {
        required string user_name= 1;
        optional int64  favorite_number = 2;
        repeated string interests= 3;

}
```
![](../../../../_Attachments/Pasted%20image%2020240125125837.png)


# References:

1. [An Intro to Protocol Buffers with Python](blog.pythonlibrary.org/2023/08/30/an-intro-to-protocol-buffers-with-python/)
2. **[Protocol Buffers Crash Course](https://www.youtube.com/watch?v=46O73On0gyI&list=PLQnljOFTspQV1emqxKbcP5esAf4zpqWpe&index=16)** (video)
3. [What are Protocol Buffers and why they are widely used?](https://medium.com/javarevisited/what-are-protocol-buffers-and-why-they-are-widely-used-cbcb04d378b6)
4. Igor Anishchenko: “Thrift vs Protocol Buffers vs Avro - Biased Comparison,” [slideshare.net](http://slideshare.net/), September 17, 2012
5. Martin Kleppmann: “[Schema Evolution in Avro, Protocol Buffers and Thrift](http://martin.kleppmann.com/2012/12/05/schema-evolution-in-avro-protocol-buffers-thrift.html),” [martin.kleppmann.com](http://martin.kleppmann.com), December 5, 2012.
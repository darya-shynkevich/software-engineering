![Pasted image 20230604134520](../../../_Attachments/Pasted%20image%2020230604134520.png)

1. ***Backward compatibility*** - Newer code can read data that was written by older code.
2. ***Forward compatibility*** - Older code can read data that was written by newer code.
#### Schema evolution
 ![Pasted image 20230818172911](../../../_Attachments/Pasted%20image%2020230818172911.png)

The translation from the in-memory representation to a byte sequence is called ***encoding*** (also known as serialization or marshalling), and the reverse is called ***decoding*** (parsing, deserialization, unmarshalling)

1. “[CWE-502: Deserialization of Untrusted Data](http://cwe.mitre.org/data/definitions/502.html),” Common Weakness Enumeration, _cwe.mitre.org_, July 30, 2014.
2. Steve Breen: “[What Do WebLogic, WebSphere, JBoss, Jenkins, OpenNMS, and Your Application Have in Common? This Vulnerability](http://foxglovesecurity.com/2015/11/06/what-do-weblogic-websphere-jboss-jenkins-opennms-and-your-application-have-in-common-this-vulnerability/),” _foxglovesecurity.com_, November 6, 2015.
3. Patrick McKenzie: “[What the Rails Security Issue Means for Your Startup](http://www.kalzumeus.com/2013/01/31/what-the-rails-security-issue-means-for-your-startup/),” _kalzumeus.com_, January 31, 2013.

For these reasons it’s generally a bad idea to use your language’s built-in encoding (`pickle`) for anything other than very transient purposes:
1. Encoding tied to programming language limits integration with other organizations.
2. Decoding process can cause security problems by instantiating arbitrary classes.
3. Versioning data is an afterthought in libraries designed for quick and easy encoding.
4. Efficiency is often not considered in encoding, leading to bad performance and bloated encoding in Java's built-in serialization.

JSON, XML, and CSV are textual formats, and thus somewhat human-readable (although the syntax is a popular topic of debate). Besides the superficial syntactic issues, they also have some subtle problems:
1. Different data encodings have issues with storing and parsing large numbers accurately, which can lead to inaccuracies in certain programming languages. JSON, for example, doesn't distinguish between integers and floating-point numbers. Moreover, there is a problem with large numbers (grater than 2^53):
		 Matt Harris: “[Snowflake: An Update and Some Very Important Information](https://groups.google.com/forum/#!topic/twitter-development-talk/ahbvo3VTIYI),” email to _Twitter Development Talk_ mailing list, October 19, 2010.
2. Binary strings cannot be stored directly in JSON or XML, so they are often encoded as text using Base64, which increases the data size by 33%.
3. XML and JSON support optional schema languages to define data types and interpretations, but these schemas can be complicated to implement and many tools don't use them, requiring applications to potentially hardcode encoding and decoding logic.
		 Francis Galiegue, Kris Zyp, and Gary Court: “[JSON Schema](http://json-schema.org/),” IETF Internet-Draft, February 2013.
4. CSV has no schema, so applications have to manually define the meaning of each row and column, and there can be issues with escaping certain characters.

#### Binary encoding

Binary formats like MessagePack and WBXML were developed to reduce the space used by JSON and XML, but they are not as popular as the textual versions. Because it’s not clear whether such a small space reduction (and perhaps a speedup in parsing) is worth the loss of human-readability.

![Pasted image 20230604145016](../../../_Attachments/Pasted%20image%2020230604145016.png)

1. [Thrift and Protocol Buffers](Protocols/Thrift%20and%20Protocol%20Buffers.md)
2. [Avro](Protocols/Avro.md)


#### Benefits of the binary encoding

1. They can be much more compact than the various “binary JSON” variants, since they can omit field names from the encoded data.
2. The schema is a valuable form of documentation, and because the schema is required for decoding, you can be sure that it is up to date (whereas manually maintained documentation may easily diverge from reality).
3. Keeping a database of schemas allows you to check forward and backward compatibility of schema changes, before anything is deployed.
4. For users of statically typed programming languages, the ability to generate code from the schema is useful, since it enables type checking at compile time.

#### Modes of Dataflow

Compatibility is a relationship between one process that encodes the data, and another process that decodes it.

- Via databases (“[Dataflow Through Databases](Dataflows/Dataflow%20Through%20Databases.md)”)
- Via service calls (“[Dataflow Through Services. REST and RPC](Dataflows/Dataflow%20Through%20Services.%20REST%20and%20RPC.md)”)
- Via asynchronous message passing (“[Message-Passing Dataflow](Dataflows/Message-Passing%20Dataflow.md)”)

# References:

1. [What is Idempotency mean?](https://medium.com/nerd-for-tech/what-is-idempotency-mean-f8eaef4229c5)
2. [Idempotency in Scalable Distributed Architectures — Example](https://medium.com/@debasisdasnospdii/idempotency-in-scalable-distributed-architectures-example-e23a12c70048)
3. “A Matrix of the Features Each Individual Language Library Supports,” [wiki.apache.org](http://wiki.apache.org/).
4. Tony Hoare: “[Null References: The Billion Dollar Mistake](http://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare),” at QCon London, March 2009.
5. Gwen Shapira: “[The Problem of Managing Schemas](http://radar.oreilly.com/2014/11/the-problem-of-managing-schemas.html),” [radar.oreilly.com](http://radar.oreilly.com), November 4, 2014.
6. Aditya Auradkar and Tom Quiggle: “[Introducing Espresso—LinkedIn's Hot New Distributed Document Store](https://engineering.linkedin.com/espresso/introducing-espresso-linkedins-hot-new-distributed-document-store),” [engineering.linkedin.com](http://engineering.linkedin.com), January 21, 2015.

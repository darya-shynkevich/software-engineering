A **loosely coupled system** is one where the different parts can interact with each other through well-defined interfaces, without having to know the internal details of the other parts.
	(+) independent variability
	(+) operational resilience
	(+) local issue can not drag the entire system down

**Coupling** describes the independent variability of connected systems, i.e., whether a change in System A affects System B. If it does, A and B are coupled **with respect to this change.**
	> Change comes in many forms, for example, a change in requirements, a change in scale, or a change in component latency or availability
	> For example, a location-dependent solution requires an update to each sender when a recipient changes location (such as a new URL or IP address). In other words, sender and receiver are coupled with respect to location, or _location coupled_ in short.

**Coupling may not be a big issue** for functional changes if you have full control over all system components, good test coverage, and high levels of automation.

> The appropriate level of coupling depends on the level of control that you have over the endpoints.

### Dimensions of Coupling

#### 1.  Technology Dependency

**Example:** Java vs C++

**Affects:** Migratability

This dependency exists when communicating systems depend on each other's implementation technology.
	> protocol
	> naming schemes
	> data types

#### 2. Location Dependency

**Example:** IP addresses, DNS

**Affects:** Composability (if one component doesn't make location assumptions about the components it is interacting with, another component such as an intermediary can take its place without any changes to the sender)

To allow systems to communicate with one another, they must agree on some common addressing scheme.

The need for sender and receiver to agree on a common address can also be eliminated by using [explicit composition via an "Assembler" component](https://www.enterpriseintegrationpatterns.com/ramblings/loanbroker_stepfunctions_pubsub.html#composition). Sender and receiver have no knowledge of each other because they are connected by a third party.
![](Pasted%20image%2020240623130120.png)

Hard-coded addresses → Host Names / URLs → Logical Names → Topics → Content → Explicit Composition

A common challenge of location decoupling is that ***logical abstractions cannot hide physical properties***. For example, if the system is location decoupled, but the new component is further away and carries more latency, then there is physical coupling, which might affect run-time characteristics.

#### 3. Data Format & Type Dependency

**Example:** Binary, XML, JSON, ProtoBuf, Avro & int16, int32, string, utf-8, null, empty

**Affects:** Integratability

*An integer number may consist of 32 bits on one system and 64 bits on another. Character strings might consist of 8-bit ASCII characters, EBCDIC values, or UNICODE characters. Therefore, both **systems have to agree on a common data representation scheme**. "Tagged" formats such as name-value pairs, XML, or JSON documents loosen up such constraints and reduce coupling.*

> **Data format change** refers to a new, renamed, or repositioned field. For example, JSON will tolerate a position change or a field addition whereas EDI records will not. 

> **Data type dependency** relates to the encoding of fields, which can include aspects such as optional fields or whether missing fields are interpreted the same as a present, but empty field

*At Google we used Protocol Buffers to describe the contract between services. Protocol Buffers specify the data format using an IDL (Interface Definition Language), from which endpoints can be generated for various languages. To make data formats backward compatible, new fields were routinely tagged as `optional` even though they were required by the receiver. The result was that coupling shifted from the IDL to the endpoint implementation.*

#### 4. Semantic Dependency

**Example:** Name, Middlename, ZIP

**Affects:** Integratability

Semantic coupling is the "next level up" from data formats and types, both from a layer communications model as well as from a complexity point-of-view. 

One system reports data weekly, another monthly, and a third one by 30-day intervals.

#### 5. Conversation Dependency

**Example:** pagination, caching, retries

**Affects:** Resilience 

Example: retry is also a conversation and is bound by its rules and assumptions: are retries allowed? is the number of retries unbounded? Is the recipient idempotent? how should the requestor handle duplicate responses resulting from overeager retries?

Systems may make assumptions about **message order** =>  If those assumptions change, or a message producer changes the order of messages, subsequent systems may have to change => coupling

#### 6. Temporal Dependency

**Example:** sync, async

**Affects:** Resilience 

**If systems communicate synchronously**, i.e., the requestor waits for the provider's response, **the requestor is temporally dependent on the provider**: a slow provider causes the requestor to also become slow. Worse yet, if the provider is unavailable or unreachable, the requesting system is also rendered unavailable.

**Asynchronous messaging solutions** cannot magically make an unavailable system available, but they **can temporally decouple systems** by having the requestor not expect an immediate response. Alternatively, the requestor could implement **_graceful degradation_**, so that it remains operational, although perhaps with limited features

### Hidden Coupling

Serverless integration services appear to be topology decoupled thanks to logical resource identifiers like ARNs on AWS. But it turns out that message formats are dependent on the source. Therefore, inserting a message queue or changing the data source in a serverless, event-driven application changes the message and forces downstream consumers to change—topology coupling!

# References:

1. ~~[The Many Facets of Coupling](https://www.enterpriseintegrationpatterns.com/ramblings/coupling_facets.html)~~
2. [Balancing Coupling in Software Design](https://www.youtube.com/watch?v=6indW7BSGZI) (video)
3. [I Made Everything Loosely Coupled. Does My App Fall Apart? • Gregor Hohpe • GOTO 2022](https://www.youtube.com/watch?v=w9a7eI6BlVc&list=PLEx5khR4g7PKxJBkaGmSDRywZ3aAZcwpK&index=7) (video)
4. [The Myth of Loose Coupling](https://dtornow225.substack.com/p/issue-45-the-myth-of-loose-coupling?r=1m9i62&utm_campaign=post&utm_medium=email&ref=blog.vvsevolodovich.dev&triedRedirect=true)
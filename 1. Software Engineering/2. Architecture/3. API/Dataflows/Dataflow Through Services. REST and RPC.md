Moreover, a server can itself be a client to another service (for example, a typical web app server acts as client to a database). This approach is often used to decompose a large application into smaller services by area of functionality, such that one service makes a request to another when it requires some functionality or data from that other service. This way of building applications has traditionally been called a *==service-oriented architecture (SOA)==*, more recently refined and rebranded as microservices architecture.

However, while databases allow arbitrary queries using the query languages, services expose an application-specific API that only allows inputs and outputs that are predetermined by the business logic (application code) of the service. *This restriction provides a degree of encapsulation: services can impose fine-grained restrictions on what clients can and cannot do.*

#### Web services

When HTTP is used as the underlying protocol for talking to the service, it is called a *==web service==*.

[Development/Encoding and Envolving/REST](Development/Encoding%20and%20Envolving/REST) is not a protocol, but rather a design philosophy that builds upon the principles of HTTP. It emphasises simple data formats, using URLs for identifying resources and using HTTP features for cache control, authentication, and content type negotiation. REST has been gaining popularity compared to SOAP, at least in the context of cross-organizational service integration,  and is often associated with microservices. 
*An API designed according to the principles of REST is called RESTful.*

By contrast, [SOAP](SOAP) is an XML-based protocol for making network API requests. Although it is most commonly used over HTTP, it aims to be independent from HTTP and avoids using most HTTP features. Instead, it comes with a sprawling and complex multitude of related standards (the web service framework, known as `WS-*`) that add various features. The API of a SOAP web service is described using an XML-based language called the Web Services Description Language, or WSDL. As WSDL is not designed to be human-readable, and as SOAP messages are often too complex to construct manually, users of SOAP rely heavily on tool support, code generation, and IDEs. 

Compared to data flowing through databases, we can make a simplifying assumption in the case of dataflow through services: *==it is reasonable to assume that all the servers will be updated first, and all the clients second. Thus, you only need backward compatibility on requests, and forward compatibility on responses.

#### The problems with remote procedure calls ([RPC](RPC)s)

All of these are based on the idea of a remote procedure call (RPC), which has been around since the 1970s. *The RPC model tries to make a request to a remote network service look the same as calling a function or method in your programming language*, within the same process (this abstraction is called location transparency). Although RPC seems convenient at first, the approach is fundamentally flawed. A network request is very different from a local function call: local function calls are predictable and under your control, while network requests are unpredictable and subject to network problems outside of your control. Local functions have consistent timing and can efficiently pass references to local memory, while network requests can experience variable latency and require encoding larger objects for transmission. Additionally, network requests may require datatype translation between programming languages.

*Custom RPC protocols have better performance than JSON over REST, but RESTful APIs have advantages like easy experimentation, wide support, and a large ecosystem of tools available.*

For these reasons, REST seems to be the predominant style for public APIs. *==The main focus of RPC frameworks is on requests between services owned by the same organization, typically within the same datacenter.==* 


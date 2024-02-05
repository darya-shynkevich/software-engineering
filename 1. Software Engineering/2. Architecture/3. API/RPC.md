**RPC** is an interprocess communication protocol that’s widely used in distributed systems. In the OSI model of network communication, RPC spans the transport and application layers.

> **Note:** The procedure or subroutine is coded as a regular/local procedure call without the programmer explicitly coding the details for the remote interaction.

# How does RPC work?

When we make a remote procedure call, the calling environment is paused and the procedure parameters are sent over the network to the environment where the procedure is to be executed.

When the procedure execution finishes, the results are returned to the calling environment where execution restarts as a regular procedure call.

To see how it works, let’s take an example of a client-server program. There are five main components involved in the RPC program, as shown in the following illustration:

![](_Attachments/Pasted%20image%2020240117233537.png)

1. A client initiates a client stub process by giving parameters as normal. The client stub is stored in the address space of the client.
2. The client stub converts the parameters into a standardized format and packs them into a message. After packing the parameter into a message, the client stub requests the local RPC runtime to deliver the message to the server.
3. The RPC runtime at the client delivers the message to the server over the network. After sending a message to the server, it waits for the message result from the server.
4. RPC runtime at the server receives the message and passes it to the server stub.

> **Note:** The RPC runtime is responsible for transmitting messages between client and server via the network. The responsibilities of RPC runtime also include retransmission, acknowledgment, and encryption.

5. The server stub unpacks the message, takes the parameters out of it, and calls the desired server routine, using a local procedure call, to do the required execution.

![](_Attachments/Pasted%20image%2020240117233732.png)
6. After the server routine has been executed with the given parameters, the result is returned to the server stub.
7. The server stub packs the returned result into a message and sends it to the RPC runtime at the server on the transport layer.
8. The server’s RPC runtime returns the packed result to the client’s RPC runtime over the network.
9. The client’s RPC runtime that was waiting for the result now receives the result and sends it to the client stub.
10. The client stub unpacks the result, and the execution process returns to the caller at this point.

# Real-world usage:

- **Google:** Google uses RPC in various parts of its distributed infrastructure. They have developed gRPC, an open-source framework that uses RPC to build efficient and highly performant distributed systems. It’s employed for services like Google Search, YouTube, etc., where it allows Google to maintain communication and data exchange between different components of their services, which is essential for achieving real-time performance.
- **Uber:** Uber uses RPC for various functions, including real-time location tracking, ride matching, and communication between drivers and riders. RPC allows Uber to provide fast and responsive services by facilitating data exchange between the user’s app and the server.
- **Facebook:** Most services at Facebook are written using Thrift for RPC, and some storage systems use Thrift for serializing records on disk. This provides several benefits to Facebook. For example, it enables interoperability between different languages, such as a Python client communicating with a C++ server.
Acts as an intermediary between external clients and the backend services or microservices within an application. This gateway simplifies the process of communication between clients and services by providing a unified entry point for clients to access various services.

Key features of an API Gateway:

- The API Gateway directs incoming API requests from clients to the appropriate backend services or microservices based on predefined rules and configurations.
- API Gateways handle user authentication and authorization, ensuring that only authorized clients can access services. They can validate API keys, tokens, or other credentials before routing requests to backend services.
- API Gateways can enforce rate limiting or client request throttling based on predefined policies.
- API Gateways can cache frequently used responses, reducing latency and minimizing the load on backend services.
- API Gateways can modify requests and responses to ensure compatibility between clients and services. This includes tasks such as converting data formats, adding or removing headers, or modifying query parameters.
- API Gateways provide features for monitoring, reporting, and troubleshooting by emitting logs, metrics, and traces.

Examples of API Gateway solutions include those provided by Microsoft Azure API Management, Amazon API Gateway, Google Apigee, and more. These services enable developers to publish, maintain, monitor, secure, and operate APIs efficiently at any scale.

![[Pasted image 20230826020815.png]]

# References:

1. ! [System Design Basics: What is an API Gateway?](https://medium.com/geekculture/system-design-basics-what-is-an-api-gateway-b858e9491608)
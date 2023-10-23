API (Application Programming Interface) - is a documented way in which external consumers can understand how they can interact with your code.
	just a contract

### Handling Large response

- **Pagination** In this method we give the responsibility to the client. The client sends an offset with the API call. The server uses this offset to figure out which data it should send next.
- **Fragmentation** This method is used when services are making API calls to other services in a microservice architecture. When there is a large response we can break the response into multiple pieces and send the pieces to the next service one by one. Each piece will have a number so the other service knows there is more information. Finally, there will be an end packet.




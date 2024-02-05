
### Functional requirements

The activities a user will be able to perform using our collaborative document editing service are listed below:

- **Document collaboration**: Multiple users should be able to edit a document simultaneously. Also, a large number of users should be able to view a document.
- **Conflict resolution**: The system should push the edits done by one user to all the other collaborators. The system should also resolve conflicts between users if they’re editing the same portion of the document.
- **Suggestions**: The user should get suggestions about completing frequently used words, phrases, and keywords in a document, as well as suggestions about fixing grammatical mistakes.
- **View count**: Editors of the document should be able to see the view count of the document.
- **History**: The user should be able to see the history of collaboration on the document.

A real-world document editor also has to have functions like document creation, deletion, and managing user access. We focus on the core functionalities listed above, but we also discuss the possibility of other functionalities in the lessons ahead.

### Non-functional Requirements

- **Latency**: Different users can be connected to collaborate on the same document. Maintaining low latency is challenging for users connected from different regions.
- **Consistency**: The system should be able to resolve conflicts between users editing the document concurrently, thereby enabling a consistent view of the document. At the same time, users in different regions should see the updated state of the document. Maintaining consistency is important for users connected to both the same and different zones.
- **Availability**: The service should be available at all times and show robustness against failures.
- **Scalability**: A large number of users should be able to use the service at the same time. They can either view the same document or create new documents.

![](../../../_Attachments/Pasted%20image%2020240127200957.png)

- **API gateway**: Different client requests will get intercepted through the API gateway. Depending on the request, it’s possible to forward a single request to multiple components, reject a request, or reply instantly using an already cached response, all through the API gateway. Edit requests, comments on a document, notifications, authentication, and data storing requests will all go through the API gateway.
- **Application servers**: The application servers will run business logic and tasks that generally require computational power. For instance, some documents may be converted from one file type to another (for example, from a PDF to a Word document) or support features like import and export. It’s also central to the attribute collection for the recommendation engine.
- **Data stores**: Various data stores will be used to fulfill our requirements. We’ll employ a relational database for saving users’ information and document-related information for imposing privilege restrictions. We can use NoSQL for storing user comments for quicker access. To save the edit history of documents, we can use a time series database. We’ll use blob storage to store videos and images within a document. Finally, we can use distributed cache like Redis and a CDN to provide end users good performance. We use Redis specifically to store different data structures, including user sessions, features for the [Typeahead Suggestion](Typeahead%20Suggestion.md) service, and frequently accessed documents. The CDN stores frequently accessed documents and heavy objects, like images and videos.
- **Processing queue**: Since document editing requires frequently sending small-sized data (usually characters) to the server, it’s a good idea to queue this data for periodic batch processing. We’ll add characters, images, videos, and comments to the processing queue. Using an HTTP call for sending every minor character is inefficient. Therefore, we’ll use WebSockets to reduce overhead and observe live changes to the document by different users.
- **Other components**: Other components include the session servers that maintain the user’s session information. We’ll manage document access privileges through the session servers. Essentially, there will also be configuration, monitoring, pub-sub, and logging services that will handle tasks like monitoring and electing leaders in case of server failures, queueing tasks like user notifications, and logging debugging information.
### Workflow

- **Collaborative editing and conflict resolution**: Each request gets forwarded to the operations queue. This is where conflicts get resolved between different collaborators of the same document. If there are no conflicts, the data is batched and stored in the time series database via session servers. Data like videos and images get compressed for storage optimisation, while characters are processed right away.
- **History**: It’s possible to recover different versions of the document with the help of a time series database. Different versions can be compared using DIFF operations that compare the versions and identify the differences to recover older versions of the same document.
- **Asynchronous operations**: Notifications, emails, view counts, and comments are asynchronous operations that can be queued through a pub-sub component like Kafka. The API gateway generates these requests and forwards them to the pub-sub module. Users sharing documents can generate notifications through this process.
- **Suggestions**: Suggestions are in the form of the [Typeahead Suggestion](Typeahead%20Suggestion.md) service that offers autocomplete suggestions for typically used words and phrases. The [Typeahead Suggestion](Typeahead%20Suggestion.md) service can also extract attributes and keywords from within the document and provide suggestions to the user. Since the number of words can be high, we’ll use a NoSQL database for this purpose. In addition, most frequently used words and phrases will be stored in a caching system like Redis.
- **Import and export documents**: The application servers perform a number of important tasks, including importing and exporting documents. Application servers also convert documents from one format to another. For example, a `.doc` or `.docx` document can be converted in to `.pdf` or vice versa. Application servers are also responsible for feature extraction for the [Typeahead Suggestion](Typeahead%20Suggestion.md) service.

# Concurrency in Collaborative Editing

## Techniques for conflict resolution

### Operational transformation

- **Causality preservation**: If operation `a` happened before operation `b`, then operation `a` is executed before operation `b`.
- **Convergence**: All document replicas at different clients will eventually be identical.

![](../../../_Attachments/Pasted%20image%2020240127211250.png)

OT has two disadvantages:
- Each operation to characters may require changes to the positional index. This means that operations are order dependent on each other.
- It’s challenging to develop and implement.

### Conflict-free Replicated Data Type (CRDT)

 A CRDT has a complex data structure but a simplified algorithm.

A CRDT satisfies both commutativity and idempotency by assigning two key properties to each character:
- It assigns a globally unique identity to each character.
- It globally orders each character.


**Note:** OT and CRDTs are good solutions for conflict resolution in collaborative editing, but our use of WebSockets makes it possible to highlight a collaborator’s cursor. Other users will anticipate the position of a collaborator’s next operation and naturally avoid conflict.

![](../../../_Attachments/Pasted%20image%2020240127211726.png)

# References:

1. [Two Ex-Google System Design Experts Compete: Who will design the better system?](https://www.youtube.com/watch?v=Zi0pPkiFemE)
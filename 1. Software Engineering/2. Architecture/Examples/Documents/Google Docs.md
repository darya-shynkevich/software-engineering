### Functional requirements:

1. Users should be able to edit documents and have their changes propagated to other users in real time
2. When other users make edits they should be propagated to us in real time
3. All users must eventually see the same state of the document
### Concurrency in Collaborative Editing

#### 1. Operational transformation

**Operational Transform**: centralised server takes in concurrent writes and transforms them as necessary before sending back to client. 
1. **Causality preservation**: If operation `a` happened before operation `b`, then operation `a` is executed before operation `b`.
2. **Convergence**: All document replicas at different clients will eventually be identical.

**Example:**
Two users, Sam and Aditi, are working on the same document, which has an initial state "BC", and they both make changes to it. Their changes will be sent to the server along with their last revision. Specifically, if Aditi inserts the character "D" at position 2 and Sam inserts the character "A" at the beginning, the server will transform Aditi's change to be inserted at the 3rd index. The server keeps track of the complete revision history of each document, allowing each collaborator to edit the document. 

![](Pasted%20image%2020240812202449.png)

**Workflow:**
1. Let’s say Bob begins the text by entering the word Hello at the top.
2. Bob keeps typing and adds the term world to his paper. John types an ‘!’ in his empty version of the document at the exact moment.
3. The edit was added to Bob’s client’s list of pending revisions. He then sent the update to the server and added it to his sent changes list.
4. John got Bob’s edit from the server and transformed it against his pending (‘!’)update using operational transformation (OT). As a result of the transition, John’s pending change was moved up five spaces to create a place for Bob’s Hello at the top of the document. When Bob and John received the notifications from the server, they both changed their last synced revision numbers to 1.
5. Following that, both Bob and John will send their pending changes to the server.
6. Because Bob’s change arrived before John’s, the server processed it first. Bob received a confirmation of the adjustment. The modification was delivered to John, who had it changed against his (‘!) change, which was still waiting.
7. The server received John’s pending modification, and John believes it should be Revision 2. The server, however, has already added Revision 2 to the revision log. The server will apply OT to John’s patch and save it as Revision 3.
8. The server started by comparing John’s transmitted change to all the other modifications committed since the last time John synced with the server. It turned John’s change against Bob’s in this example (‘Google’ at 6). As a result, John’s change over-index was pushed by 6. This shift is identical to John’s client’s metamorphosis when he first received Bob’s (‘Hello’ at 1).

**Disadvantages:**
1. Each operation to characters may require changes to the positional index. This means that **operations are order dependent** on each other.
2. It’s challenging to develop and implement.
#### 2. Conflict-free Replicated Data Type (CRDT)

CRDTs are when we can have data live on multiple servers such that when we eventually merge the CRDTs, all of the nodes converge on the same data.

**Idea:** we want to avoid having to send each write through a single server before sending it off to client, or else that server will become a bottleneck for us. No every single server needs to see every single write. 
##### State vs. Operation CRDT

###### 2.1 State CRDT:
- Send full text document
- Have a merge function *(will merge two documents into one)* that is commutative, associative and idempotent
- Problematic for big documents
###### 2.2 Operation CRDT:
- Just send the "operation" -> "Index x at position y"
- Need a merge function that is commutative and associative
- Idempotent? Need to include additional metadata for each operation
	- It assigns a globally unique identity to each character.
	- It globally orders each character.
- How to we make character insertion commutative? 

**Operation CRDT Merge Function**

How can we make it so that the order of our writes do not matter and we still get the same result?

![](Pasted%20image%2020240812193308.png)

![](Pasted%20image%2020240812193410.png)

**Example of problem**: interleaving (when we try to insert two words with different length at the same place in document)

![](Pasted%20image%2020240812193618.png)

**Idempotence**
How can we ensure that if we receive a write as a client, we do not process it more than once?
1. Each write has associated UUID: works but now all of a sudden each client has to store an extra UUID per character type
2. A [version vector](Version%20vector.md): if version vector of write is smaller than ours, we have seen it

![](Pasted%20image%2020240812195601.png)

- All changes will be in DB before a single client can see them
- So if the Client has the last change `C4` and suddenly recieved change `C8` => the Client knows that changes `C5`, `C6` and `C7` are in the DB and the Client can fetch missed changes. 

**New clients:**
When a new client starts up, it needs to fetch the whole document.

![](Pasted%20image%2020240812200107.png)

=> Solution - [[Change Data Capture]]

![](Pasted%20image%2020240812200443.png)

Having two tables allows us much greater performance if we need to partition. Imagine a 100 page document, now we can partition. 

**Documents Polling**

![](Pasted%20image%2020240812200826.png)

1. New client subscribe for the changes 
2. New client request Document from Document Snapshot DB
3. New client receives `C5`
4. New client poll `C4` from Writes DB

**We can use just a Document Snapshot DB**

We can remove `step 4` and use Document Snapshot DB. 

Eventually will be correct if we just write to a single document DB. However, then it may become a bottleneck. 
We could also have each server write to its own partition, but then a client that wants to read a document has to read from many DB nodes.
##### High Level Design 

![](Pasted%20image%2020240812203055.png)
#### 3. Natural conflict avoidance 

OT and CRDTs are good solutions for conflict resolution in collaborative editing, but the usage of WebSockets makes it possible to highlight a collaborator’s cursor. Other users will anticipate the position of a collaborator’s next operation and naturally avoid conflict.

# References:

1. ! ~~[Design Google Docs/Real Time Text Editor | Systems Design Interview Questions With Ex-Google SWE](https://www.youtube.com/watch?v=YCjVIDv0zQY) (video)~~
2. [Two Ex-Google System Design Experts Compete: Who will design the better system?](https://www.youtube.com/watch?v=Zi0pPkiFemE)
3. ~~[Design Google Docs](https://www.enjoyalgorithms.com/blog/design-google-docs)~~
4. [Collaborative Text Editing with Eg-walker: Better, Faster, Smaller](https://arxiv.org/pdf/2409.14252)
5. [Software Architect Interview: Designing Google Docs | System Design Mock Interview](https://www.youtube.com/watch?v=y9K-6XkvSgQ)
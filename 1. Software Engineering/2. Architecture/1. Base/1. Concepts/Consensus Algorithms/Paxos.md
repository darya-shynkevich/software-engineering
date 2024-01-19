This algorithm guarantees that the system will come to an agreement on a single value, and tolerate the failure of any number of nodes (potentially all of them), as long as more than half the nodes work properly at any time, which is a significant improvement.
### Roles

The Paxos algorithm defines three different roles:
- Proposers: responsible for proposing values (potentially received from clients’ requests) to the _acceptors_ and trying to persuade them to accept their value to arrive at a common decision.
- Acceptors: responsible for receiving these proposals and replying with their decision on whether this value can be chosen or not.
- Learners: responsible for learning the outcome of the consensus, storing it (in a replicated way) and potentially acting on it, by either notifying clients about the result or performing actions.

Every node in the system can potentially play multiple roles.
### Phases

The Paxos algorithm is split into two phases, each of which contains two parts:
#### Phase 1 (a)
A _proposer_ selects a number N and sends a prepare request with this number prepare(N) to at least a majority of the _acceptors_.

N is the **round Identifier**, which has two interesting properties:

1. The round identifier has to be bigger than any previous round identifier used by any other proposer in our Paxos cluster. This is achieved by incrementing a counter `i++`.
2. Make round identifiers unique because we never want two proposers to come up with the same round identifier and reuse it. Reusing identifiers would break the protocol. To achieve this we append `node_number` to the counter.

! We will use a function `cat(i++, node_number)` that appends `node_number` to the counter `i++`.

#### Phase 1 (b)
When receiving a prepare request, an _acceptor_ has the following options:

- If it has not already responded to another prepare request of a higher number N, it responds to the request with a promise not to accept any more proposals that are numbered less than N. It also returns the highest-numbered proposal it has accepted, if any (Note: the definition of a proposal follows).
- Otherwise, if it has already accepted a prepare request with a higher number, it rejects this prepare request. This ideally gives a hint to the proposer about the number of the other prepare request it has already responded to.

#### Phase 2 (a)
If the _proposer_ receives a response to its prepare(N) requests from a majority of _acceptors_, then it sends an accept(N, v) request to these _acceptors_ for a proposal numbered N with a value v. The value is selected according to the following logic:

- If any of the _acceptors_ had already accepted another proposal and included that in its response, then the _proposer_ uses the value of the highest-numbered proposal among these responses. Essentially, this means that the _proposer_ ***attempts to bring the latest proposal to conclusion.***
- Otherwise, if none of the _acceptors_ had accepted any other proposal, then the _proposer_ is free to select any desired value. This value is usually selected based on the clients’ requests.

#### Phase 2 (b)
If the _acceptor_ receives an accept(N, v) request for a proposal numbered N, it accepts the proposal, unless it has already responded to a prepare(k) request of a higher number (k>N).

Furthermore, as the _acceptors_ accept proposals, they also announce their acceptance to the _learners_. When a _learner_ receives an acceptance from a majority of _acceptors_, it knows that a value has been chosen. This is the most basic version of the Paxos protocol.

### Example

As an example, we can observe that the _proposers_ can play the role of _learners_ as well, since they receive some of these accept responses anyway, minimize traffic, and improve the **performance** of the system.

During Phase 1 (a) of the protocol, the _proposers_ have to select a proposal number N. These numbers must be unique for the protocol to maintain its **correctness** properties. This is so that _acceptors_ are always able to compare two prepare messages. This can be achieved in several ways, but the easiest one is to compose these numbers out of two parts, the one being an integer and the second one being a unique identifier of the proposer (i.e., the IP address of the node). In this way, proposers can draw numbers from the same set.

As we have insinuated at the beginning of this section, multiple _proposers_ can initiate concurrent prepare requests. The proposer that receives a response to its prepare request from a majority of _acceptors_ is essentially elected as the current (but temporary) leader.

As a result, it can proceed with making a proposal request. The value of this proposal will be the chosen one unless a majority of _acceptors_ have failed (and did not reply to the proposal) or another leader stepped up, becoming the temporary leader in the meanwhile (in which case, the _acceptors_ will reject this proposal).

A majority _[[Quorum]]_ consists of more than half of the nodes of the system, such as at least k+1 nodes in a system of 2k nodes.

This protocol guarantees that there can’t be two different proposers that complete both phases of the protocol concurrently because proposers require a majority quorum to proceed with a proposal. As a result, only a single value can be chosen, satisfying the agreement property of consensus.

# References:

1. [Paxos Basic Case Explained](https://medium.com/@the.york.wei/paxos-basic-case-explained-db366dad7462)
2. [Distributed Consensus : PAXOS](https://medium.com/@logeshrajendran/paxos-a9d76ebf04f3)
3. [Paxos algorithm](https://martinfowler.com/articles/patterns-of-distributed-systems/paxos.html)
4. [The Part-Time Parliament](https://lamport.azurewebsites.net/pubs/lamport-paxos.pdf) - Leslie Lamport
5. [Paxos Made Simple](https://lamport.azurewebsites.net/pubs/paxos-simple.pdf) - Leslie Lamport
6. [Paxos Made Live - An Engineering Perspective](https://static.googleusercontent.com/media/research.google.com/en/us/archive/paxos_made_live.pdf) - Chandra et al
7. [Revisiting the Paxos Algorithm](https://groups.csail.mit.edu/tds/paxos.html) - Lynch et al
8. [How to build a highly available system with consensus](http://bwl-website.s3-website.us-east-2.amazonaws.com/58-Consensus/Acrobat.pdf) - Butler Lampson
9. [Reconfiguring a State Machine](https://www.microsoft.com/en-us/research/publication/reconfiguring-a-state-machine/) - Lamport et al - changing cluster membership
10. [Implementing Fault-Tolerant Services Using the State Machine Approach: a Tutorial](https://citeseer.ist.psu.edu/viewdoc/summary?doi=10.1.1.20.4762) - Fred Schneider
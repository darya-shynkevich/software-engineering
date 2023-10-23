## [[Advantages of Modular Monolith]]

# Full comparison
## 1. Separation of Concerns

- **Modular Monolith:** Within a modular monolith, we achieve ***a high degree of separation but within the confines of a single application***. Think of it as ***rooms in a house***. Every module, like our hypothetical ‘user reviews’ segment, is a unique room. While they are part of the same building, they serve distinct purposes and maintain their boundaries.
- **Microservices:** Microservices elevate this concept, treating each concern as not just a separate room, but an ***entirely different building***. The ‘user reviews’ would be its standalone service, with its runtime, database, and potentially even its tech stack

## 2. Communication Mechanisms

- **Modular Monolith:** Module communication is internal, making it ***inherently faster***. 
- **Microservices:** Here, services ***communicate over the network***. It allows for a diverse tech stack and independent deployments but can introduce latency.

## 3. Data Management

- **Modular Monolith**: The ‘share nothing’ idea is particularly striking within a modular monolith. Each module operates with its dedicated data store, data and business models, all contained within the overarching application. In practice, this might mean ***in terms of data using a single PostgreSQL instance where each module has its separate schema, ensuring no data overlap***.
  ![[Pasted image 20231019225639.png]]
- **Microservices**: The ‘share nothing’ approach in microservices is more about ***complete operational independence***. Each microservice naturally has its separate data store, ensuring that it operates without any direct dependency on others. However, while this gives each service more autonomy, it can introduce challenges, especially when you need transactional consistency across services. This is because each service’s datastore is not just separate in principle, as with modular monoliths, but often physically distinct, sometimes even residing on separate servers or environments.

## 4. Architectural Alignment

- **Modular Monolith:** Given its contained nature, it’s often ***simpler to align architectural practices and enforce consistency***. It does mean, however, that the ***entire system might need to be redeployed for changes in a single module***.
- **Microservices:** Services can be developed, deployed, and scaled independently. It’s the epitome of **separation of concerns** but can lead to challenges in maintaining uniformity and can complicate deployment pipelines.
## [[Conformist Pattern]] in Modular Monoliths

## Flexibility in Microservices

However, it’s essential to strike a balance. While flexibility is a strength, without proper governance, it can lead to a fragmented system where maintaining uniformity and integration becomes a challenge.


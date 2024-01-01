Moving from a monolith architecture to microservices is complex, but sometimes worth it.

1. Is the new service being used by any other service excluding the original service?
2. Is there sufficient decoupling between the original and new service?
3. Is the business/technical requirements of the new service separate from the original service?


![[Pasted image 20231019143429.png]]

# Monoliths advantages

1. ***Monoliths are better for small teams***: microservice architecture is more complex. It is might a waste of time for small teams to deal with the complexity of the microservices architecture. For small teams, a unified application is more manageable in comparison to splitting code up into microservices.
2. ***Generally better performance (no latency)***
3. Since the code and expectations in monoliths are in one place it is ***much harder to have breaking changes***

# Reasons for migration?

1. ***Separation of concerns***: in a microservice architecture we can make changes to one service without affecting other services. However in monoliths, since all our code is in one place making changes will affect the entire system.
2. ***Engineering is easier***: doubtable 
3. ***Easy deployments***: doubtable, but generally true
4. ***Better scalability*** if different parts of the system scales differently: Each service can be scaled independently. So it is more cost-effective and saves time compared to monoliths.
5. ***Great for large teams***: Microservice is a great choice when we have a large team. Each team can work on a service that makes the development process much more efficient.
6. ***More flexibility*** is you really need it: Each service can be written in a different language and tech stack. This allows teams to pick up a tech stack that is the most appropriate.

# How are we going to make a smooth migration?

#### 1. Rewrite it from scratch.
Route some of our users (let’s say 20%) to our new architecture. If it works fine then we can route more and more users until we route all users to the new architecture.

*The problem with this approach is the huge engineering challenge. We need to write all microservices, make sure databases are correctly configured, and then redirect users. So we need to invest a lot of resources in the very beginning.*

#### 2. Do monolith modularisation

**[[Modular Monolith]]:** It’s essentially a ***monolithic architecture where the application is divided into loosely coupled modules***. Each module focuses on a distinct concern, ensuring separation of concerns. 

[[Modular Monolith vs. Microservices]]

Whenever we want to add a new module to our application, instead of adding it to the monolith we can make it a single service. 

*This approach is better because we need less investment in the beginning and the migration will be much smoother.*




# References: 

1.  [Migrating from a Monolith to Microservices](https://medium.com/@interviewready/migrating-from-a-monolith-to-microservices-4921caf47d10)
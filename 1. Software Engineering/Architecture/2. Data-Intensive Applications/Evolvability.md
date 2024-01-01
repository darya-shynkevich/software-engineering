*Software maintenance and evolution are characterised by their huge cost and cumbersome implementation.*

***Evolvability*** - *an attribute that bears on the ability of a system to accommodate changes in its requirements throughout the system’s lifespan with the least possible cost while maintaining architectural integrity.*

![Pasted image 20230818102632](../../../_Attachments/Pasted%20image%2020230818102632.png)

#### Measuring attributes

1. Analyzability: modularity, complexity, and documentation
2. Integrity: architectural documentation
3. Changeability: complexity, coupling, change impact, encapsulation, reuse, modularity
4. Portability: mechanisms facilitating adaptation to different environments
5. Extensibility: modularity, coupling, encapsulation, change impact
6. Testability: complexity, modularity
7. Domain-specific attributes: depend on the specific domains

![Pasted image 20230818103547](../../../_Attachments/Pasted%20image%2020230818103547.png)

*The evaluation results included (i) the identified and prioritized requirements on the software architecture; (ii) identified components/modules that need to be refactored for enhancement or adaptation; (iii) refactoring investigation documentation which describes the current situation and solutions to each identified candidate that need to be refactored, including estimated workload; and (iv) test scenarios.*

***Analyzability*** was addressed through refining activities for each identified requirement. ***Integrity*** was addressed through extracting rationale for each design decision; and providing training, guidelines and code examples for software developers and using tactics that enable the achievement of a certain quality characteristic. ***Changeability*** was addressed through restructuring the original function-oriented architecture to product-line architecture. ***Extensibility*** was addressed through the definition of a Base Software SDK, consisting of well documented API, wizards and tools for developing application-specific extensions. ***Portability*** was handled through the portability layer which encapsulates infrastructure technology choices and provides interfaces for application software in the controller. ***Testability*** was addressed through defining test scenarios and applications to support platform testing. ***Domain-specific attribute*** was planned with respect to functionality partition of the controller software.

==***The best known quality models for evaluating quality include McCall, Boehm, FURPS, ISO 9126 and Dromey***==


## References:

1. [Analyzing Software Evolvability](http://www.es.mdh.se/pdf_publications/1251.pdf )
2. [API Evolution Is a Challenge. Could Contract Testing Be the Solution?](https://tech.ebayinc.com/engineering/api-evolution-with-confidence-a-case-study-of-contract-testing-adoption-at-ebay/)
An imperative language tells the computer to perform certain operations in a certain order. You can imagine stepping through the code line by line, evaluating conditions, updating variables, and deciding whether to go around the loop one more time.

![[Pasted image 20230629002456.png]]
In a declarative query language, like SQL or relational algebra, you just specify the pattern of the data you want — what conditions the results must meet, and how you want the data to be transformed (e.g., sorted, grouped, and aggregated) — but not how to achieve that goal. It is up to the database system’s query optimizer to decide which indexes and which join methods to use, and in which order to execute various parts of the query.

![[Pasted image 20230629002518.png]]

A declarative query language is attractive because it is typically more concise and easier to work with than an imperative API. But more importantly, it also hides implementation details of the database engine, which makes it possible for the database system to introduce performance improvements without requiring any changes to queries.

*The SQL example doesn’t guarantee any particular ordering, and so it doesn’t mind if the order changes. But if the query is written as imperative code, the database can never be sure whether the code is relying on the ordering or not. **The fact that SQL is more limited in functionality gives the database much more room for automatic optimizations.***

***Finally, declarative languages often lend themselves to parallel execution.*** Today, CPUs are getting faster by adding more cores, not by running at significantly higher clock speeds than before. 
1. *Imperative code* is very hard to parallelize across multiple cores and multiple machines, because it *specifies instructions that must be performed in a particular order*. 
2. *Declarative languages have a better chance of getting faster in parallel execution because they specify only the pattern of the results, not the algorithm that is used to determine the results.* The database is free to use a parallel implementation of the query language, if appropriate.

![[Pasted image 20230629003611.png]]

### Declarative Queries on the Web

![[Pasted image 20230629002925.png]]

Here the CSS selector `li.selected > p` declares the pattern of elements to which we want to apply the blue style.

Imagine what life would be like if you had to use an imperative approach. In JavaScript, using the core Document Object Model (DOM) API, the result might look something like this.

![[Pasted image 20230629003826.png]]








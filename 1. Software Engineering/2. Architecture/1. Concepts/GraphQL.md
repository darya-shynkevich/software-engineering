1. Exposing a **query language** to untrusted clients **increases the attack surface** of the application.
	1. Every object returned and/or field resolved, your authorisation system is called to confirm that the current user has access.
2. There is no limit to how big a query can be. Even in a completely empty schema, the types exposed for [introspection](https://graphql.org/learn/introspection/) are cyclical, so its possible to craft a valid query that returns MBs of JSON
	1. Estimate the complexity of resolving **every single field in the schema**, and abandon queries that exceed some maximum complexity value
	2. Capture the actual complexity of the run query and take it out of bucket of credits that resets at some interval
	3. [Prevent deeply nested queries](https://graphql-ruby.org/queries/complexity_and_depth#prevent-deeply-nested-queries).
3. **Query parsing**: its not enough to just limit the payload size, as you will have valid queries that are larger than the the smallest dangerous malicious query.
	1. If your server exposes a concept of maximum number of errors to accrue before abandoning parsing, [this can be mitigated](https://github.com/rmosolgo/graphql-ruby/issues/4154).
4. **Performance**: incompatibility with HTTP caching + data fetching and the N+1 problem. It can **become** a problem with no backend changes when a client modifies a query
5. **Complexity**: the various mitigations to security and performance issues we’ve gone through add **significant** complexity to a codebase.

# References:

1. ~~[Why, after 6 years, I’m over GraphQL](https://bessey.dev/blog/2024/05/24/why-im-over-graphql/)~~
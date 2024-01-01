
1. **Say goodbye to DRY**
   What to do with common code?
	- Have a common library?
	- How does the common library get updated? Keep different versions everywhere?
	- Force updates regularly, creating dozens of pull requests across all repositories?
	- Keep it all in a monorepo? That comes with its _own_ set of problems.
	- Allow for some code duplication?
	- Forget it, each team gets to reinvent the wheel every time.

2. **Developer ergonomics will crater.** 
   
   “Developer ergonomics” is the friction, the amount of effort a developer must go through in order to get something done, be it working on a new feature or resolving a bug.
   
   With microservices, an engineer has to have a mental map of the entire system in order to know what services to bring up for any particular task, what teams to talk to, whom to talk to, and what about. ***The `Tracing` tools can help a bit with mind map***
   
	- Developer privileges in GitHub/GitLab
	- Default environment variables and configuration
	- CI/CD
	- Code quality checkers
	- Code review settings
	- Branch rules and protections
	- Monitoring and observability
	- Test harness
	- Infrastructure-as-code

3. **Integration tests**
   
   Who is in charge of the overall integration test suite, in Postman or wherever else? Is there one?
   
   Integration testing a distributed setup is a nearly-impossible problem, so we pretty much gave up on that and replaced it with another one - `Observability`. Just like “microservices” are the new “distributed systems”, “observability” is the new “debugging in production”.
   
![Pasted image 20231019142917](../../../../_Attachments/Pasted%20image%2020231019142917.png)



# References: 

1. [Death by a thousand microservices](https://renegadeotter.com/2023/09/10/death-by-a-thousand-microservices.html)
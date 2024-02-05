
1. One codebase tracked in revision control, many deploys
2. All dependencies should be declared, with no implicit reliance on system tools or libraries.
3. Configuration that varies between deployments should be stored in the environment.
4. All backing services are treated as attached resources and attached and detached by the execution environment.
5. The delivery pipeline should strictly consist of build, release, run.
6. Applications should be deployed as one or more stateless processes with persisted data stored on a backing service.
7. Self-contained services should make themselves available to other services by specified ports.
8. Concurrency is advocated by scaling individual processes.
9. Fast startup and shutdown are advocated for a more robust and resilient system.
10. All environments should be as similar as possible.
11. Applications should produce logs as event streams and leave the execution environment to aggregate.'
12. Any needed admin tasks should be kept in source control and packaged with the application.

![Pasted image 20230602085402](../../_Attachments/Pasted%20image%2020230602085402.png)

# References:

1. https://architecturenotes.co/12-factor-app-revisited/
2. [12 Factor App Revisited](https://architecturenotes.co/12-factor-app-revisited/)
3. [Distributed System: Understanding 12 Principles of the 12-Factor App Methodology](https://medium.com/@bindubc/distributed-system-understanding-12-principles-of-the-12-factor-app-methodology-9da4b504a195)


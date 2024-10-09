
#### Why I Still Use Python Virtual Environments in Docker

1. Predictability : It’s _good_ to use the same tools and primitives in development and in production.
2. Predictability : **if** you a) never install anything globally, **and** you b) use the `python` binary from the virtual environment **while** passing it `-I`, you **know** everything that’s not in the standard library **must** be in the virtual environment. That makes Python’s import behavior much more **predictable** and debugging import issues less of a murder mystery.

# References:

1. ! [50 вопросов по Docker, которые задают на собеседованиях, и ответы на них](https://habr.com/ru/company/southbridge/blog/528206/)
2. [The evolution from virtual machines to containers](https://www.youtube.com/watch?v=8qU3hZOXlBE&list=PLQnljOFTspQWsD-rakNw1C20c1JI8UR1r&index=13) (video)
3. [What are Docker, Containers, Virtual Machines, and Containerization?](https://medium.com/javarevisited/what-are-docker-containers-virtual-machines-and-containerization-e68bf076edf4)
4. [Docker Networking Crash Course](https://www.youtube.com/watch?v=OU6xOM0SE4o&list=PLQnljOFTspQWsD-rakNw1C20c1JI8UR1r&index=20) (video)
5. [Docker Volumes Explained (PostgreSQL example)](https://www.youtube.com/watch?v=G-5c25DYnfI&list=PLQnljOFTspQWsD-rakNw1C20c1JI8UR1r&index=14)
6. [Разбираемся с Docker: как создаются образы](https://habr.com/ru/companies/southbridge/articles/701950/)
7. [Lifecycle of Docker Container](https://medium.com/@BeNitinAgarwal/lifecycle-of-docker-container-d2da9f85959)
8. [Docker Best Practices for Python Developers](https://testdriven.io/blog/docker-best-practices/)
9. [Методики уменьшения размеров образов Docker](https://habr.com/ru/company/ruvds/blog/485650/)
10. [Несколько советов о том, как ускорить сборку Docker-образов](https://habr.com/ru/company/itsumma/blog/501680/)
11. ! ~~[Why I Still Use Python Virtual Environments in Docker](https://hynek.me/articles/docker-virtualenv/)~~
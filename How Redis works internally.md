
> - Do you know what are the key features to make it so fast?
> - Internally it is using [[SkipList]] with a double list for [[LRU]], a very efficient data structure which makes it fast
> - Very good, what else? How does it handle incoming connections?
> - It is socket based server, listen and accept the connection then start a thread to process the incoming data”
> - Ok, how about 10k connections at the time? Redis will open 10k threads?

One of the key features of making Redis fast is because its I/O model. As a single-threaded application still fast, which solved the [c10k issue](https://en.wikipedia.org/wiki/C10k_problem#:~:text=The%20C10k%20problem%20is%20the,concurrently%20handling%20ten%20thousand%20connections.) in a smart way — by using the Epoll model.

[[Alternative IO models]]


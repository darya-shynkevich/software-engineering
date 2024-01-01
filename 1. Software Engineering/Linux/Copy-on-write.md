Copy-on-write is a computer science technique where, instead of creating a new copy of data when it is being modified, the computer only creates a new copy when the data is actually changed. Here's an example: Imagine you have a big bowl of your favorite candy that you want to share with your friends. Instead of giving each friend their own bowl of candy, you give them each a spoon. When your friends want to take some candy, they use their spoons to scoop some out of the main bowl. This way, you don't have to make several copies of the candy bowl and can save space.  

In computer science, this technique is often used in situations where many processes are accessing the same data. Instead of creating a new copy of the data for each process, the computer only creates a new copy if the data is actually changed by one of the processes. This can save on memory usage and improve performance.  

A verifiable fact about copy-on-write is that it is used in the Linux operating system's file system, where it is called "COW" for short. This technique is used to create snapshots of the file system, allowing for easy backups and restoring of data.

## References:

1. https://en.wikipedia.org/wiki/Copy-on-write
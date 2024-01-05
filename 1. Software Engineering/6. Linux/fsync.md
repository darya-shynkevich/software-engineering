***fsync**() transfers ("flushes") all modified in-core data of (i.e., modified buffer cache pages for) the file referred to by the file descriptor `_fd_` to the disk device (or other permanent storage device) so that all changed information can be retrieved even if the system crashes or is rebooted.

*For various, reasons when changes are made to file they are done to caches and calls to `fsync()` ensure they are persisted to disk and accessible later.*

#### *Explainlikeimfive.io:*

*In computer science, `fsync` is a function that ensures that all data that is written to a file on a computer's hard drive is actually saved to the drive. Think of it like making sure that all your homework is saved before turning off your computer.*

*For example, imagine you are writing a document and you hit "save." The computer may tell you that the file has been saved, but it could still be in the computer's memory and not yet written to the hard drive. If you turn off the computer before the data is actually saved to the hard drive, you could lose all your work. `Fsync` makes sure that the data is saved to the hard drive before you turn off the computer.*

*Overall, `fsync` is an important function in computer science to make sure that data is properly saved to a hard drive, preventing loss of valuable information.*

# References:

1. [Your SSD lies but that's ok .. I think | Postgres fsync](https://www.youtube.com/watch?v=JK2ZIx8jRu4) (video)
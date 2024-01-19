*==It is a storage solution strategically designed to oversee and facilitate access to files and directories across numerous servers, nodes, or machines, which are often spread out across a network.==*

![Pasted image 20230826023923](../../../../_Attachments/Pasted%20image%2020230826023923.png)

The core objective of a distributed file system is to empower users and applications to interact with and manage files just as they would on a local file system, even though the real location of these files might be on remote servers.

(+) by distributing files across multiple servers, a distributed file system bolsters fault tolerance.
(+) the distribution of files across various servers enhances availability. Even if certain servers are offline, other servers can continue to furnish data.
(+) distributed file systems can heighten performance by enabling data access from the nearest server to the requesting client.
(+) as the demand for data storage escalates, additional servers can be integrated into the distributed file system to accommodate growing requirements.
(+) files can be duplicated across multiple servers, assuring redundancy and thwarting data loss in the event of server breakdowns.

# References:

1. [**Object Storage**](http://www.datacenterknowledge.com/archives/2013/10/04/object-storage-the-future-of-scale-out)
2. [HDFS vs Cloud-based Object storage(S3)](https://luminousmen.com/post/hdfs-vs-cloud-based-object-storage-s3)
3. [Scaling HDFS at Uber](https://eng.uber.com/scaling-hdfs/)
4. [Reasons for Choosing S3 over HDFS at Databricks](https://databricks.com/blog/2017/05/31/top-5-reasons-for-choosing-s3-over-hdfs.html)
5. [File System on Amazon S3 at Quantcast](https://www.quantcast.com/blog/quantcast-file-system-on-amazon-s3/)
6. [Image Recovery at Scale Using S3 Versioning at Trivago](https://tech.trivago.com/2018/09/03/efficient-image-recovery-at-scale-using-amazon-s3-versioning/)
7. [Cloud Object Store at Yahoo](https://yahooeng.tumblr.com/post/116391291701/yahoo-cloud-object-store-object-storage-at)
8. [Ambry: Distributed Immutable Object Store at LinkedIn](https://www.usenix.org/conference/srecon17americas/program/presentation/shenoy)
9. [Dynamometer: Scale Testing HDFS on Minimal Hardware with Maximum Fidelity at LinkedIn](https://engineering.linkedin.com/blog/2018/02/dynamometer--scale-testing-hdfs-on-minimal-hardware-with-maximum)
10. [Hammerspace: Persistent, Concurrent, Off-heap Storage at Airbnb](https://medium.com/airbnb-engineering/hammerspace-persistent-concurrent-off-heap-storage-3db39bb04472)
11. [MezzFS: Mounting Object Storage in Media Processing Platform at Netflix](https://medium.com/netflix-techblog/mezzfs-mounting-object-storage-in-netflixs-media-processing-platform-cda01c446ba)
12. [Magic Pocket: In-house Multi-exabyte Storage System at Dropbox](https://blogs.dropbox.com/tech/2016/05/inside-the-magic-pocket/)
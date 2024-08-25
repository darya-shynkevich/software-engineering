
# [File Upload](File%20Upload.md)

# **Direct byte stream upload for small files**

Works well for files < 100 mb

Direct byte stream upload for small files involves reading the file and converting it into a continuous stream of bytes. These bytes are then transmitted to the server using standard communication protocols like HTTP or FTP, where the server reassembles them into the original file. *==This method is highly efficient for small files due to minimal additional data or metadata, reducing transmission overhead and minimizing latency.==* It’s a straightforward and simple approach, making it ideal for scenarios where simplicity is essential. The mechanism is as follows: ***the file is read sequentially, and each byte is sent to the server in a continuous stream, allowing the server to reconstruct the original file.***

# **Multipart Upload**

##### **Steps:**
1. **File Division —** The original file is divided into smaller parts, often of equal or configurable size.
2. **Sequential Upload** — These parts are uploaded one by one to the server or storage system, usually in parallel or concurrently.
3. **Combination** — Once all parts are uploaded, the server combines them to reconstruct the original file.

##### **Scenarios**:
1. **Large File Upload:** Uploading large files like high-definition videos or extensive datasets in one piece can be difficult and error-prone. By breaking them into smaller parts, the upload process becomes more efficient and reliable.
2. **Unreliable Network Conditions:** It minimizes the risk of data loss caused by network interruptions. Since each part can be uploaded and verified independently, any transmission issues affecting one part can be resolved without impacting the entire upload process.

# **Breakpoint Resumable Upload**

Resumable Upload is a file transfer mechanism that allows the transfer of large files to be paused and resumed from where it left off, rather than starting the transfer from the beginning in the event of a network failure or interruption.
##### **Steps:**
1. **File Division** — Divide files into parts or chunks for upload.
2. **Multipart Upload Initialization** — Start a multipart upload session on the server.
3. **Data Block Transmission** — Send each part to the server, either serially or in parallel.
4. **Network Interruption Handling —** If a network failure or interruption occurs during the transfer, only the part that was being transferred at that moment needs to be retransmitted.
5. **Recording Progress** — The client keeps track of the progress of the transfer, recording which parts have been successfully transferred and which are pending.
6. **Resuming Upload** — When the network connection is restored or the transfer is resumed, the client can request the server to start the transfer from the point where it was interrupted.
7. **Completion Check** — Ensure all uploaded data is complete and intact, then reconstruct the file.
##### **Implementation**:
 1. Divide the files into data blocks of the same size following specific splitting rules: *The client (front-end) should fragment the file into fixed-size chunks and provide the fragment serial number and size in requests to the server.*
 2. Initialize a multipart upload task and receive a unique identifier for this multipart upload: *The server creates a configuration file (conf file) to record chunk locations. The length of this conf file equals the total number of fragments.*
 3. Send each fragmented data block according to a chosen strategy (serial or parallel): *When a chunk is uploaded, it writes a value of 127 to the conf file. Unuploaded locations default to 0, and uploaded locations are set to Byte.MAX_VALUE 127.*
 4. Once the transmission is complete, the server checks if the uploaded data is complete and then combines the data blocks to reconstruct the original file: *The server calculates the starting position based on the fragment sequence number in the request data and the size of each fragment (assuming a fixed and consistent fragment size) and writes the file fragment data using the read file fragment data.*

During the implementation of multipart upload, coordination between the front-end and back-end is crucial. Both parties need to ensure that factors such as the file size and the number of upload blocks are consistent to prevent upload issues. Additionally, traditional file-related operations often necessitate the establishment of a file server infrastructure, which may involve using solutions like FastDFS or HDFS.

# References:

1. [Strategies for Efficient File Uploads](https://jinlow.medium.com/strategies-for-efficient-file-uploads-a9aada439559)
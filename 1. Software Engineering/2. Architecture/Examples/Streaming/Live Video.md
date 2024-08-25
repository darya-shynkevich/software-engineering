![](Pasted%20image%2020240529223122.png)
Real-time messaging protocol (**[RTMP](https://en.wikipedia.org/w/index.php?title=Real-Time_Messaging_Protocol)**) is used to send live video from the `broadcaster` to the Facebook infrastructure. It’s based on TCP and splits the live video into separate audio and video streams.

`Live stream server` transcoded live video into different bit rates.

Then they create `1-second video segments` in [MPEG-DASH](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP) format and send those to viewers.

**DASH** is a streaming protocol over HTTP. It consists of a manifest file and media files. Imagine the manifest file as a collection of pointers to media files. They update the manifest file whenever new video segments are created. Hence the viewer can request the media files in the correct order.

## Live Streaming Architecture

### 1. Scalability

> There’s a risk of the live stream server failing while processing the video.

![](Pasted%20image%2020240529223429.png)

Facebook scales `live stream servers` and uses **[consistent hashing](https://systemdesign.one/consistent-hashing-explained/)** to select one. It automatically selects the next server if the active server fails.

 `Stream ID` = `consistent hash key` =>  broadcaster could reconnect to the same server if their network connection drops.

### 2. Network Bandwidth

> Viewers may have different network bandwidths as people use mobile internet.

**Adaptive bitrate** streaming =  a live video gets broken down into smaller parts. And each part gets converted into different resolutions = the video quality changes based on network bandwidth.

"+" measure the upload bandwidth of the broadcaster. And then change the video resolution to match the bandwidth.

### 3. Latency

> Viewers from another continent may experience latency due to their distance from the live stream server.

Set up extra servers in a `2-level hierarchy`. 

`Edge servers` - closer to the viewers. It stores the live video segments for low latency.

`Origin servers` -  to reduce the live stream server load.

Imagine each continent has an origin server, while each country has many edge servers.

![](Pasted%20image%2020240529224024.png)

So when a viewer requests a video segment, they serve it from the edge server if it got cached. Otherwise the request gets forwarded to the origin server and live stream server.

### 4. Thundering Herd

An edge server can handle 200k requests per second. Thus reducing the traffic reaching the origin server.

> But if many people watch a live video concurrently, there’s a risk of the [thundering herd problem](https://en.wikipedia.org/wiki/Thundering_herd_problem) in the edge server.

![](Pasted%20image%2020240529224131.png)

=> `edge server` should have an HTTP proxy and a **caching layer**.

This means the viewer first checks whether a specific video segment is in the edge server cache.

The HTTP proxy then requests the origin server for that video segment only if it’s not in the cache.

Also they store video segments in different cache servers within an edge server to load balance the traffic.

Besides they built the origin server with the same architecture. And it forwards the request to the live stream server on a cache miss.

Then the video segment gets cached in each server to handle future requests.

! Yet statistically 2 out of 100 requests will reach the origin server => **request coalescing** = all concurrent requests for a specific video segment get stored in a queue. While only a single request gets forwarded to the server.

Then all the requests in the queue get that video segment at once after the server responds. Thus reducing the server load and preventing the thundering herd problem.

Also many edge servers may concurrently request a specific video segment from an origin server. So they do request coalescing at the origin server to reduce the live stream server load.

Besides they replicate the video segments across edge servers for better performance.

# References:

1. ~~[How Facebook Scaled Live Video to a Billion Users](https://newsletter.systemdesign.one/p/live-streaming-architecture?utm_source=substack&publication_id=1511845&post_id=144760801&utm_medium=email&utm_content=share&utm_campaign=email-share&triggerShare=true&isFreemail=true&r=1vxw4z&triedRedirect=true)~~

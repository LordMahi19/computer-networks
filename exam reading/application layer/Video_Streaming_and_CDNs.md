# Video Streaming and CDNs

Video streaming is the largest consumer of Internet bandwidth. Delivering high-quality video to a massive, global audience presents significant challenges.

### Video Characteristics
- **High Bit Rate:** Video, even when compressed, requires a high and sustained throughput.
- **Compression:** Video is highly compressed to reduce its size.
    - **Spatial Coding:** Compressing individual images (frames).
    - **Temporal Coding:** Only sending the differences between consecutive frames.
- **Bit Rate Types:**
    - **Constant Bit Rate (CBR):** Video is encoded at a fixed rate.
    - **Variable Bit Rate (VBR):** The bit rate changes depending on the complexity of the video at any given moment.

### Streaming Challenges
- **Heterogeneity:** Users have a wide range of devices (phones, TVs) and network connections (WiFi, 5G, fiber) with different bandwidth capabilities.
- **Jitter:** Network delays are variable. This variation, called jitter, can disrupt the continuous playout of a video.
- **Solution: Client-side Buffering:** The client application buffers a few seconds of video before starting playback. This buffer absorbs variations in network delay, ensuring a smooth viewing experience.

### DASH (Dynamic, Adaptive Streaming over HTTP)
DASH is the industry standard for streaming video. It allows the client to dynamically adapt the video quality to match the available network bandwidth.

- **Server Side:**
    1.  The video file is encoded into several different versions, each at a different bit rate (quality level).
    2.  Each version is broken into small chunks (e.g., 2-10 seconds long).
    3.  The server creates a **manifest file** that contains the URLs for all the chunks of all the different versions.

- **Client Side:**
    1.  The client first requests the manifest file.
    2.  The client then intelligently and adaptively requests the video chunks.
    3.  It continuously measures the available network bandwidth.
    4.  Based on the bandwidth, it decides which version (bit rate) of the next chunk to request.
        - If bandwidth is high, it requests a high-quality chunk.
        - If bandwidth drops, it requests a lower-quality chunk to avoid buffering stalls.
    - This entire process is done by the client, making the server simple and scalable (it's just a standard HTTP server).

### Content Distribution Networks (CDNs)
To solve the problem of delivering content to a global audience, streaming services like Netflix and YouTube use **Content Distribution Networks (CDNs)**.

- **Core Idea:** A CDN is a geographically distributed network of servers that stores copies of content. When a user requests content, they are directed to the CDN server that is closest to them.
- **Benefits:**
    - **Reduced Latency:** Users receive content from a nearby server, which lowers delay.
    - **Higher Throughput:** The user is likely to have a better network path to a nearby server.
    - **Scalability:** Spreads the load across many servers, preventing any single server from being overwhelmed.
- **CDN Strategies:**
    - **Enter Deep:** Place many smaller server clusters deep into access networks around the globe (e.g., Akamai).
    - **Bring Home:** Place fewer, larger server clusters at key locations, closer to major ISP interconnection points (e.g., Limelight).
- **CDN Operation:**
    1.  When you visit a site like `netflix.com`, your browser makes a DNS query.
    2.  The DNS system is configured to redirect your request to a special CDN DNS server.
    3.  The CDN DNS server sees your IP address and returns the IP address of the CDN content server that is "best" for you (usually the one that is geographically closest).
    4.  Your browser then streams the video directly from that optimal CDN server.

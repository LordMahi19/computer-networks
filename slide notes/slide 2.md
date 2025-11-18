The topics discussed in "slide 2.pdf," which focuses on the **Application Layer** of computer networks, cover both the foundational principles of application development and detailed examinations of key Internet protocols.

Here is a comprehensive list of every topic mentioned in the document:

### I. Application Layer: Overview and Goals

- **Chapter/Course Context:** Slides are adopted from material by J.F Kurose and K.W. Ross's _Computer Networking: A Top-Down Approach_ 8th edition.
- **Our Goals:** Understanding the conceptual and implementation aspects of application-layer protocols.
- **Specific Areas of Study:** Transport-layer service models, client-server paradigm, peer-to-peer paradigm, and programming network applications using the socket API.
- **Protocols/Infrastructure Examined:** HTTP, SMTP, IMAP, DNS, video streaming systems, and CDNs.

### II. Principles of Network Applications

- **Network Applications:** Examples of network applications, including social networking, Web, text messaging, e-mail, multi-user network games, streaming stored video (YouTube, Hulu, Netflix), P2P file sharing, Voice over IP (Skype), real-time video conferencing (Zoom), Internet search, and remote login.
- **Application Creation:** Writing programs that run on different end systems and communicate over the network. Network-core devices do not run user applications.
- **Client-Server Paradigm:** The structure involving **servers** (always-on host, permanent IP, often in data centers) and **clients** (intermittently connected, dynamic IPs, do not communicate directly with each other). Examples include HTTP, IMAP, and FTP.
- **Peer-to-Peer (P2P) Architecture:** Characterized by **no always-on server** where arbitrary end systems (peers) communicate directly. Peers provide service in return for requesting service (self scalability). Management can be complex due to intermittent connectivity and changing IP addresses. Example: P2P file sharing (BitTorrent).
- **Processes and Sockets:**
    - **Process:** A program running within a host. Processes communicate across hosts by exchanging messages.
    - **Client/Server Process:** Client initiates communication; server waits to be contacted.
    - **Sockets:** The interface (door) between the application process and the end-end transport protocol.
- **Addressing Processes:** Identification requires both the **IP address** of the host and **port numbers** associated with the process (e.g., HTTP server port 80, mail server port 25).
- **Application-Layer Protocol Definition:** Defines the types of messages exchanged (request, response), message **syntax** (fields and delineation), message **semantics** (meaning of fields), and rules for sending/responding to messages.
- **Protocol Types:** **Open protocols** (defined in RFCs, allowing interoperability, e.g., HTTP, SMTP) versus **proprietary protocols** (e.g., Skype, Zoom).
- **Transport Service Requirements:** The needs of applications regarding:
    - **Data Integrity:** Reliable data transfer (e.g., file transfer) vs. loss tolerance (e.g., audio).
    - **Timing:** Low delay requirement (e.g., Internet telephony, interactive games).
    - **Throughput:** Minimum throughput requirement (e.g., multimedia) vs. **elastic apps** (use whatever is available).
    - **Security:** Encryption and data integrity.
- **Internet Transport Protocols:**
    - **TCP Service:** Reliable transport, flow control, congestion control, connection-oriented. Does not provide timing, minimum throughput, or security guarantees.
    - **UDP Service:** Unreliable data transfer, connectionless. Does not provide reliability, flow control, congestion control, timing, throughput guarantee, or security.
- **Securing TCP:** Transport Layer Security (TLS) provides encrypted TCP connections, data integrity, and end-point authentication, typically implemented in the application layer using TLS libraries.

### III. Web and HTTP

- **Web Objects:** Web pages consist of objects (HTML, images, etc.), each addressable by a URL (host name and path name).
- **HTTP Overview:** Hypertext Transfer Protocol operates on a client/server model. It uses TCP port 80. **HTTP is stateless**—the server maintains no information about past client requests.
- **HTTP Connections:**
    - **Non-persistent HTTP:** One object sent per TCP connection, which is then closed. Requires **$2\text{RTT} + \text{file transmission time}$** per object.
    - **Persistent HTTP (HTTP 1.1):** Multiple objects can be sent over a single open TCP connection, reducing RTTs.
- **HTTP Message Format:** Messages are ASCII (human-readable format).
    - **Request Message:** Contains a request line (GET, POST, HEAD, PUT methods) and header lines.
    - **Methods:** **GET**, **POST** (data in entity body), **HEAD** (requests headers only), and **PUT** (uploads new file/object).
    - **Response Message:** Contains a status line (protocol, status code, status phrase) and header lines.
    - **Status Codes:** Examples include **200 OK**, **301 Moved Permanently**, **400 Bad Request**, **404 Not Found**, and **505 HTTP Version Not Supported**.
- **Cookies:** Used by websites and browsers to maintain state between stateless transactions.
    - **Components:** Cookie header in HTTP response, cookie header in subsequent requests, cookie file on host, and a back-end database at the Web site.
    - **Usage:** Authorization, shopping carts, recommendations, and user session state.
    - **Privacy:** Third-party persistent cookies (tracking cookies) allow common identity tracking across multiple sites.
    - **GDPR:** Relates cookies that can identify an individual to personal data.
- **Web Caches (Proxy Servers):** Satisfy client requests without involving the origin server.
    - **Benefits:** Reduce response time, reduce traffic on the access link, and enable content delivery.
    - **Performance:** Performance gains are calculated based on cache hit rate and access link utilization.
- **Conditional GET:** Used to avoid sending an object if the browser has an up-to-date cached version, using the `If-modified-since` header and receiving a `304 Not Modified` status if the copy is current.
- **HTTP/2:** Designed to decrease delay in multi-object requests. It addresses Head-of-Line (HOL) blocking (where a small object waits behind a large one) found in HTTP 1.1 FCFS scheduling. HTTP/2 uses client-specified object priority, server push, and dividing objects into frames to mitigate HOL blocking.
- **HTTP/3:** Adds security, per-object error, and congestion control over **UDP** (using QUIC).

### IV. E-mail: SMTP and IMAP

- **Components:** User agents, mail servers, and Simple Mail Transfer Protocol (SMTP).
- **Mail Servers:** Contain a user mailbox for incoming messages and a message queue for outgoing mail.
- **SMTP:** Used between mail servers to send email. It uses **TCP port 25** for reliable transfer.
    - **Transfer Phases:** Handshaking, transfer of messages, and closure.
    - **Interaction:** ASCII command/response interaction (e.g., HELO, MAIL FROM, RCPT TO, DATA, QUIT).
    - **Model:** SMTP uses a **client push** model, contrasting with HTTP's client pull.
    - **Message Format (RFC 2822):** Defines the syntax including header lines (To, From, Subject) and the body (ASCII characters only).
- **Mail Access Protocols:** Used for retrieving mail from the receiver's server.
    - **IMAP (Internet Mail Access Protocol):** Messages stored on the server, providing retrieval, deletion, and folder management.
    - **HTTP:** Used by web-based services (e.g., Gmail) to access mail.

### V. The Domain Name System (DNS)

- **Purpose:** To map human-readable domain names (e.g., cs.umass.edu) to 32-bit IP addresses.
- **Structure:** A **distributed database** implemented in a hierarchy of name servers. It is a core Internet function implemented as an application-layer protocol.
- **DNS Services:** Hostname-to-IP-address translation, host aliasing, mail server aliasing, and load distribution.
- **Hierarchy:** Consists of Root DNS Servers, Top-Level Domain (TLD) servers (.com, .edu), and Authoritative DNS servers.
- **Local DNS Name Servers:** Cache mappings and forward requests into the DNS hierarchy for resolution.
- **Query Types:**
    - **Iterated Query:** The contacted server replies with the name of the next server to contact.
    - **Recursive Query:** The contacted server takes the burden of resolving the name.
- **Caching:** Improves response time, but cached entries (TTL) may be out-of-date.
- **DNS Records (RRs):** Stored in the database, formatted as `(name, value, type, ttl)`.
    - **Types:** **A** (hostname to IP address), **CNAME** (alias to canonical name), **NS** (domain to authoritative server name), **MX** (value is name of SMTP mail server).
- **Security:** Concerns include DDoS attacks (bombarding servers) and Spoofing attacks (DNS cache poisoning), requiring DNSSEC for authentication.

### VI. P2P Applications (BitTorrent)

- **File Distribution Comparison:** P2P is generally more efficient than client-server distribution for a large number ($N$) of users because new peers bring new upload capacity ($u_s + \Sigma u_i$).
- **BitTorrent Architecture:** Files are divided into chunks. A **tracker** tracks peers participating in the **torrent** (group of peers exchanging chunks).
- **Chunk Exchange:** Peers request missing chunks using the **rarest first** strategy.
- **Tit-for-Tat:** Sending chunks is governed by this mechanism: Alice sends chunks to the four peers currently sending her chunks at the highest rate. Other peers are "choked".
- **Optimistic Unchoke:** Every 30 seconds, a random peer is selected to receive chunks, giving them a chance to join the top four.

### VII. Video Streaming and CDNs

- **Video Dominance:** Streamed video consumes a major portion of Internet bandwidth (e.g., Netflix, YouTube).
- **Challenges:** Scaling to billions of users and handling **heterogeneity** (different device/bandwidth capabilities).
- **Video Characteristics:** Video is a sequence of images. Compression uses spatial and temporal coding to decrease the number of bits required. Video can be Constant Bit Rate (CBR) or Variable Bit Rate (VBR).
- **Streaming Challenges:** Network delays are variable (jitter), requiring a client-side buffer to ensure continuous playout timing.
- **DASH (Dynamic, Adaptive Streaming over HTTP):**
    - **Server Function:** Divides video files into multiple chunks, each encoded at multiple different rates, and provides a manifest file with chunk URLs.
    - **Client Function:** Periodically estimates bandwidth and requests the chunk encoding rate that is sustainable.
- **Content Distribution Networks (CDNs):** Geographically distributed sites store multiple copies of videos to stream content efficiently.
    - **Strategies:** **Enter deep** (many servers close to users, e.g., Akamai) or **Bring home** (fewer large clusters near access nets, e.g., Limelight).
    - **Netflix:** Uses its own OpenConnect CDN nodes.
    - **Access Steps:** CDN usage involves DNS redirection (using CNAME records) to locate the optimal CDN server for streaming.

### VIII. Socket Programming

- **Goal:** To learn how to build client/server applications that communicate using sockets.
- **Two Socket Types:** UDP (unreliable datagram) and TCP (reliable, byte stream-oriented).
- **UDP Sockets:** Connectionless; no handshaking is required. The sender must explicitly attach the IP destination address and port number to each packet.
- **TCP Sockets:** Client must contact the running server; a connection (handshake) is required. The server creates a **new socket** for communication with that specific client, allowing it to handle multiple clients simultaneously. TCP provides a reliable, in-order byte-stream transfer ("pipe").
- **Code Examples:** Provided for creating and managing sockets in both UDP and TCP scenarios using Python.
- **Timeouts:** Techniques for handling timeouts using the `settimeout()` function and `try-except` blocks are introduced, crucial for reliable data transfer protocols.
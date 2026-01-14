# 1. What is the difference between network architecture and application architecture? 

**Network Architecture** is the infrastructure that enables communication. **Application Architecture** is the software logic that performs tasks.

Think of it like a city:

- **Network Architecture** is the **roads, traffic lights, and highways**. It decides how traffic (data) moves from place to place.
    
- **Application Architecture** is the **buildings (banks, stores, homes)**. It decides what happens inside (processing money, selling goods) once you arrive.
    

### Quick Comparison

|**Feature**|**Network Architecture**|**Application Architecture**|
|---|---|---|
|**Focus**|**Connectivity:** Moving data reliably between devices.|**Functionality:** Processing data to solve a user problem.|
|**Components**|Routers, switches, firewalls, bandwidth, VPNs.|Databases, user interfaces (UI), APIs, code logic.|
|**Key Question**|"How do I get this message from Server A to Server B?"|"What does the software _do_ with the message once it arrives?"|


# 2. What information is used by a process running on one host to identify a process running on another host?

To identify a specific process running on another host, the sending process uses a combination of two pieces of information, often collectively called a **Socket Address**:

1. **The IP Address** (identifies the **host**)1
    
2. **The Port Number** (identifies the **process**)2
    

### Breakdown of the Information

|**Component**|**Purpose**|**Analogy**|
|---|---|---|
|**IP Address**|Uniquely identifies the **computer (host)** on the network where the target process is running.|The **Street Address** of an apartment building.|
|**Port Number**|Uniquely identifies the **specific application or service** running on that computer.|The **Apartment Number** of the specific resident.|

### Why not use the Process ID (PID)?

While the operating system assigns a **Process ID (PID)** to every running program internally, this ID is random and changes every time the program restarts. It is meaningless to outside computers.

Instead, a process "binds" to a specific **Port Number** (e.g., Web Servers usually bind to Port 80).3 The operating system maintains a table mapping that Port Number to the current internal PID, allowing the network data to reach the correct application.

---

### Key Term: The Socket

When you combine the IP Address and the Port Number (and the protocol, like TCP or UDP), the resulting combination is called a **Socket**.

- **Example:** `192.168.1.5:80`
    
    - `192.168.1.5` = The Host
        
    - `80` = The Process (Web Server)
        

... [Sockets | IP Address With Port Number](https://www.youtube.com/watch?v=7NAElB5nv20) ...

This video is relevant because it visually explains how sockets combine IP addresses and port numbers to enable process-to-process communication.


# 3. What is meant by a handshaking protocol?
A **handshaking protocol** is a process where two network entities exchange control messages to establish a connection before any application data is transferred.

### Core Functions

- **Initialization:** It sets up connection variables, such as starting sequence numbers.
- **Agreement:** Entities use the handshake to agree on parameters like receive buffer sizes and to confirm their mutual willingness to communicate.
- **State Setup:** It transitions the connection into an "established" state.

### Key Examples

- **TCP 3-way Handshake:** The standard process for connection-oriented transport, involving three steps: **SYN**, **SYNACK**, and **ACK**.
- **SMTP Handshaking:** The initial "greeting" phase of an email transfer where servers identify themselves using commands like "HELO".
- **QUIC Handshake:** A more efficient modern approach that establishes reliability, security, and congestion control states in a single round-trip time (RTT).

### Context and Analogies

Protocols like **TCP** are "connection-oriented" because they require this setup, whereas **UDP** is "connectionless" because it transmits data with **no handshaking**. The sources compare this to human interactions, such as two people saying "Hi" before starting a conversation or climbers using the "On belay? / Belay on / Climbing" sequence to ensure safety before proceeding.

# 4. What does stateless protocol mean? Is IMAP stateless? What about SMTP?
A **stateless protocol** is one in which the server maintains **no information about past client requests**. In such protocols, every request is treated as independent of previous ones, meaning the server does not need to track multi-step exchanges or the "state" of a transaction. Protocols that maintain state are generally more complex because they must reconcile views of that state if a client or server crashes.

Regarding **IMAP** and **SMTP**:

- **HTTP** is explicitly defined by the sources as a **stateless protocol**.
- The provided sources **do not explicitly state** whether **IMAP** is stateless or stateful. They do describe it as a protocol where messages are stored on a server, providing functions for retrieval, deletion, and folder management.
- The sources **do not explicitly categorize SMTP** as stateless or stateful. However, they describe SMTP as having a multi-phase process including **handshaking**, **message transfer**, and **closure**, which involves a command/response interaction.

_Information from outside the sources (which you may want to independently verify):_ In general networking terminology, **IMAP** and **SMTP** are considered **stateful** protocols because they maintain session information (such as the current state of a mailbox or the progress of an email transaction) during a connection.

# 5. Why are MX records needed? Would it not be enough to use a CNAME record? (Assume the email client looks up email addresses through a Type A query and that the target host only runs an email server.)

A **stateless protocol** (such as HTTP) is one where the server maintains **no information about past client requests**, treating each exchange as independent. The sources identify **IMAP and SMTP** as email application-layer protocols but **do not explicitly categorize** them as stateless or stateful.

**MX records** are required to provide **mail server aliasing**, which maps a domain name to the specific hostname of its mail server. While a **CNAME** provides a general alias for a canonical name, an **MX record** is the specialized DNS resource type used specifically to identify which host handles SMTP traffic for a domain.

_Information from outside the sources:_ In standard networking, **IMAP and SMTP are stateful** because they maintain session information (like login states or the progress of a multi-step email transfer) during a connection.

# 6. Besides network-related considerations such as delay, loss, and bandwidth performance, there are other important factors that go into designing a CDN server selection strategy. What are they? 


Beyond standard network performance, CDN server selection strategies involve several key factors:

- **Content Availability:** CDNs must decide which specific content to place in which nodes to bring services close to end users.
- **Load Distribution:** Distributed databases like **DNS** are used to map a single name to multiple IP addresses, balancing traffic across replicated servers.
- **Redirection Mechanisms:** Selection is managed via **manifest files** that provide URLs for content chunks or through DNS **CNAME** records that redirect clients to a CDN’s authoritative server.
- **Geographic Deployment:** Strategies include "entering deep" by placing servers directly in **access networks** or "bringing home" by housing large clusters in **Points of Presence (POPs)** near those networks.
- **Client-Side Intelligence:** Protocols like **DASH** allow the client to dynamically choose coding rates and server locations based on its own bandwidth estimations and needs.
- **IP Protocol Compatibility:** CDNs often use **dual-stack (IPv4 and IPv6)** options to ensure connectivity across diverse network environments and meet regulatory requirements.


# 7. Consider an HTTP client that wants to retrieve a Web document at a given URL. The IP address of the HTTP server is initially unknown. What transport and application-layer protocols besides HTTP are needed in this scenario? 


In this scenario, to retrieve a Web document from a URL when the IP address is initially unknown, the HTTP client must utilize **DNS** at the application layer and both **UDP** and **TCP** at the transport layer.

### Application-Layer Protocols

- **DNS (Domain Name System):** The client first requires DNS to translate the human-readable **hostname** found in the URL (such as `www.someschool.edu`) into a **32-bit IP address**. DNS is an application-layer protocol implemented as a distributed database of name servers that facilitates this name-to-address translation.
- **DHCP (Dynamic Host Configuration Protocol):** Although the prompt focuses on retrieving a specific document, the client end-system often relies on DHCP to obtain its own **IP address**, the address of its **first-hop router**, and the IP address of its **DNS server**. DHCP is a "plug-and-play" application-layer protocol used when a host joins a network.

### Transport-Layer Protocols

- **UDP (User Datagram Protocol):** DNS typically runs over **UDP**. UDP is a "no-frills" transport protocol that does not require a connection setup, which avoids the RTT (Round Trip Time) delay that would be incurred by TCP, making it efficient for the simple query/reply interactions used by DNS. Similarly, DHCP messages are encapsulated in UDP.
- **TCP (Transmission Control Protocol):** Once the IP address is resolved via DNS, the client uses **TCP** to support the actual HTTP communication. HTTP is a "reliable data transfer" application that requires a 100% reliable connection to ensure the document is received without errors. The client must initiate a **TCP connection** (typically to port 80) with the server before any HTTP request can be sent. TCP provides essential services for Web document retrieval, including in-order delivery, flow control, and congestion control.

### Summary of the Protocol Interaction

1. The client identifies the **hostname** from the URL.
2. The client sends a **DNS query** (Application Layer) encapsulated in a **UDP segment** (Transport Layer) to find the server's IP address.
3. Once the IP address is returned, the client initiates a **TCP connection** (Transport Layer) to the server.
4. After the TCP connection is established, the client sends the **HTTP GET request** (Application Layer) over that connection to retrieve the document.


# question 8
```python
Consider the following string of ASCII characters that were captured by Wireshark when the browser sent an HTTP GET message (i.e., this is the actual content of an HTTP GET message). The characters _<cr><lf>_ are carriage return and line-feed characters (that is, the italized character string _<cr>_ in the text below represents the single carriage-return character that was contained at that point in the HTTP header). Answer the following questions, indicating where in the HTTP GET message below you find the answer.[P4]

GET /cs453/index.html HTTP/1.1_<cr><lf>_Host: gai a.cs.umass.edu_<cr><lf>_User-Agent: Mozilla/5.0 ( Windows;U; Windows NT 5.1; en-US; rv:1.7.2) Gec ko/20040804 Netscape/7.2 (ax) _<cr><lf>_Accept:ex t/xml, application/xml, application/xhtml+xml, text /html;q=0.9, text/plain;q=0.8,image/png,*/*;q=0.5 PRObLEMs 199 _<cr><lf>_Accept-Language: en-us,en;q=0.5_<cr><lf>_AcceptEncoding: zip,deflate_<cr><lf>_Accept-Charset: ISO -8859-1,utf-8;q=0.7,*;q=0.7_<cr><lf>_Keep-Alive: 300_<cr> <lf>_Connection:keep-alive_<cr><lf><cr><lf>_  


1. What is the URL of the document requested by the browser? 
    

2. What version of HTTP is the browser running? 
    

3. Does the browser request a non-persistent or a persistent connection? 
    

4. What is the IP address of the host on which the browser is running? 
    

5. What type of browser initiates this message? Why is the browser type needed in an HTTP request message?
```

Based on the provided HTTP GET message and the sources, here are the answers to your questions:

- **URL of the document requested:** The path of the requested document is found in the **request line** (the first line), and the host is found in the **Host header** (the second line). Combining these, the URL is **`http://gaia.cs.umass.edu/cs453/index.html`**.
- **HTTP version:** The browser is running **HTTP/1.1**. This information is located at the end of the **request line** (the first line).
- **Persistent or non-persistent connection:** The browser requests a **persistent connection**. This is found in the **`Connection: keep-alive`** header line at the end of the message. A persistent connection allows multiple objects to be sent over a single TCP connection between the client and server.
- **IP address of the host:** The IP address of the host is **not contained within this HTTP GET message**. This message is an application-layer protocol message. IP addresses are 32-bit identifiers found in the **IP datagram header** at the network layer, which encapsulates the transport-layer segment containing this HTTP message.
- **Browser type and necessity:** The message was initiated by **Netscape/7.2**. This information is found in the **`User-Agent:` header** (the third line). The browser type is included so the server can identify the client's software; this allows the server to send different versions of the same object to different types of browsers to ensure compatibility or optimal display. (Note: The specific reason for the browser type is standard networking knowledge and not explicitly detailed in the sources, though they highlight the existence of various browser types like Safari and Firefox).


# 9. The UDP server was described needed only one socket, whereas the TCP server needed two sockets. Why? If the TCP server were to support n simultaneous connections, each from a different client host, how many sockets would the TCP server need?

A **UDP server** requires only **one socket** because it is **connectionless**; it receives all incoming datagrams through a single "door" and extracts the sender's address from each packet.

In contrast, a **TCP server** is **connection-oriented** and uses two types of sockets: a **welcoming socket** to accept initial requests and a dedicated **connection socket** created for each specific client to maintain a unique "pipe".

If a TCP server supports **$n$ simultaneous connections**, it would need **$n + 1$ sockets**:

- **$n$ connection sockets** (one for each active client).
- **1 welcoming socket** (to continue listening for new requests).



# question 10

```python
Suppose Bob joins a BitTorrent torrent, but he does not want to upload any data to any other peers (so called free-riding).    

1. Alice claims that he can receive a complete copy of the file that is shared by the swarm. Is Alice's claim possible? Why or why not?  
    

2. Alice further claims that he can further make his “free-riding” more efficient by using a collection of multiple computers (with distinct IP addresses) in the computer lab in his department. How can he do that? [P26]
```

Alice's claim is **possible**, and her suggestion for using multiple computers can indeed make "free-riding" more efficient based on how the BitTorrent protocol functions.

### How Free-Riding is Possible in BitTorrent

While BitTorrent is designed around a **"tit-for-tat"** mechanism to encourage cooperation, a peer can still receive a complete file without uploading due to a process called **optimistic unchoking**.

1. **The Tit-for-Tat Rule:** Normally, a peer (like Alice) sends file chunks only to a limited number of other peers (typically the top four) who are currently providing data to her at the highest rate. All other peers are "choked," meaning they do not receive data from her.
2. **Optimistic Unchoking:** To allow new peers to discover better trading partners and to ensure the swarm remains dynamic, every 30 seconds, a peer will **randomly select** one additional peer to "unchoke" and send chunks to, regardless of whether that peer has uploaded anything in return.
3. **Bob's Strategy:** If Bob joins the swarm and refuses to upload, he will be choked by most peers most of the time. However, eventually, different peers in the swarm will **randomly unchoke** him optimistically. Over a long enough period, Bob can accumulate all the 256Kb chunks of the file solely through these random acts of "optimistic unchoking" from various members of the swarm.

### Making Free-Riding More Efficient with Multiple IP Addresses

Alice’s claim that using multiple computers with distinct IP addresses increases efficiency is based on increasing the probability of being selected for the random rewards mentioned above.

- **Increasing the "Lottery" Chances:** In a BitTorrent swarm, peers are generally identified by their IP addresses and ports. If Bob uses ten different computers with distinct IP addresses, the tracker and other peers will view him as **ten separate, independent peers**.
- **Multiplying Optimistic Unchokes:** Since each peer in the swarm randomly selects a peer to unchoke every 30 seconds, having ten "Bobs" in the swarm instead of one **multiplies Bob's chances** of being selected for an optimistic unchoke at any given time.
- **Aggregation:** Each of Bob's computers will independently receive different chunks of the file through these random unchokes. Bob can then collect all the chunks from his various computers in the lab to assemble the complete file much faster than he could with a single machine.

By operating as multiple distinct entities, Bob effectively exploits the protocol's openness to new peers to bypass the reciprocal requirements of the tit-for-tat system.



# 11. We have seen that Internet TCP sockets treat the data being sent as a byte stream but UDP sockets recognize message boundaries. What are one advantage and one disadvantage of byte-oriented API versus having the API explicitly recognize and preserve application-defined message boundaries? 

Based on the sources provided, the distinction between the byte-stream nature of TCP and the message-oriented nature of UDP leads to the following advantage and disadvantage for a byte-oriented API (TCP):

### **Advantage: Reliability and Simplified Data Flow**

One major advantage of a byte-oriented API is that it provides a **reliable, in-order "pipe" abstraction** for data transfer. Because the protocol treats data as a continuous stream of bytes, it handles the complexities of **reordering segments, flow control, and congestion control** automatically. This allows the application to send large amounts of data without worrying about individual packet sizes or the specific mechanics of how those bytes are chunked for transmission across the network.

### **Disadvantage: Lack of Inherent Message Delineation**

The primary disadvantage of a byte-oriented API is that it has **no inherent "message boundaries"**. Because the transport layer does not recognize where one application message ends and the next begins, the **application developer is responsible for implementing the syntax** to delineate messages.

In a message-oriented API (like UDP), a single send operation typically corresponds to a single receive operation that preserves the original message boundary. In contrast, with a byte-oriented API like TCP:

- A single `recv()` call might return only a **fragment of a message** or parts of **multiple messages** combined, depending on how the data arrived in the stream.
- Applications must use their own rules, such as **fixed-length headers, special delimiters (like CRLF in SMTP), or length fields**, to manually reconstruct application-defined messages from the continuous stream of bytes.
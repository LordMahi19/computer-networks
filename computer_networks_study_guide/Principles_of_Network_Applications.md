# Principles of Network Applications

This note covers the core architectural paradigms and requirements of network applications.

### Application Architectures

1.  **Client-Server Architecture:**
    - **Server:** An always-on host with a permanent IP address that waits for and services requests from clients. Servers are often located in data centers.
    - **Client:** An end system that communicates with the server. Clients are intermittently connected, may have dynamic IP addresses, and typically do not communicate directly with other clients.
    - **Examples:** Web (HTTP), Email (SMTP), File Transfer (FTP).

2.  **Peer-to-Peer (P2P) Architecture:**
    - There is no always-on, central server. Instead, arbitrary end systems (called **peers**) communicate directly with each other.
    - **Self-Scalability:** Peers both request service and provide service. New peers bring new service capacity to the system.
    - **Challenges:** Management is more complex due to the intermittent connectivity and changing IP addresses of peers.
    - **Example:** P2P file sharing (BitTorrent).

### Processes and Sockets

- **Process:** A program running within a host. Processes on different hosts communicate by exchanging **messages**.
- **Socket:** A software interface that acts as a "door" through which a process sends and receives messages to/from the network. It provides the API (Application Programming Interface) between the application layer and the transport layer.

### Addressing Processes
To send a message to a specific process on a remote host, an identifier must be specified that includes:
1.  **IP Address:** The 32-bit (IPv4) or 128-bit (IPv6) address that uniquely identifies the host on the network.
2.  **Port Number:** A 16-bit number that identifies the specific process (via its socket) running on that host.
    - **Well-known ports:** e.g., HTTP server uses port 80, SMTP mail server uses port 25.

### Application-Layer Protocols
An application-layer protocol defines how an application's processes, running on different end systems, pass messages to each other. It specifies:
- **Message Types:** The types of messages exchanged (e.g., request, response).
- **Message Syntax:** The structure of the messages, including the fields and how they are delineated.
- **Message Semantics:** The meaning of the information in the fields.
- **Rules:** The rules for when and how a process sends and responds to messages.
- **Open vs. Proprietary:** Protocols can be open (publicly available, like HTTP) or proprietary (specific to a company, like Skype).

### Transport Service Requirements
Applications have different needs from the transport layer:

| Requirement         | Description                                                                                             | Examples                               |
| ------------------- | ------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| **Data Integrity**   | Some apps require 100% reliable data transfer.                                                          | File transfer, web transactions.       |
| **Loss Tolerance**  | Some apps can tolerate some data loss.                                                                  | Streaming audio/video.                 |
| **Timing**          | Some apps require low delay to be effective.                                                            | Interactive games, real-time video.    |
| **Throughput**      | Some apps require a minimum amount of throughput to be effective (**bandwidth-sensitive**).               | Multimedia streaming.                  |
| **Elastic Apps**    | Apps that can make use of as much, or as little, throughput as happens to be available.                   | Email, file transfer.                  |
| **Security**        | Many apps require confidentiality and data integrity.                                                   | E-commerce, online banking.            |

### Internet Transport Protocols
The Internet provides two main transport protocols:

1.  **TCP (Transmission Control Protocol):**
    - **Service:** Reliable, connection-oriented data transfer. Provides flow control and congestion control.
    - **Does NOT provide:** Timing guarantees, minimum throughput guarantees, or security.

2.  **UDP (User Datagram Protocol):**
    - **Service:** Unreliable, connectionless "best-effort" data transfer.
    - **Does NOT provide:** Reliability, flow control, congestion control, timing, throughput guarantees, or security.

**Securing TCP:** **Transport Layer Security (TLS)** is a protocol that runs in the application layer and uses TCP to provide encrypted, secure communication. It offers data integrity and end-point authentication.

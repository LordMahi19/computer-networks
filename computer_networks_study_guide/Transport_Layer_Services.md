# Transport Layer Services

The transport layer sits between the application layer and the network layer, providing essential services for process-to-process communication.

### Logical Communication
- **Role:** While the network layer provides **host-to-host** communication (delivering packets from one computer to another), the transport layer provides **process-to-process** logical communication.
- **Analogy:** Imagine two houses, one for the Ann family and one for the Bill family. The postal service (the network layer) delivers mail from house to house. A person inside each house (the transport layer) then collects the mail and delivers it to the specific person (process) it's addressed to (e.g., Ann or Bill).

### Transport Protocol Actions
- **Sender Side:** The transport layer takes messages from the application layer, breaks them into smaller chunks called **segments**, and passes these segments to the network layer.
- **Receiver Side:** The transport layer receives segments from the network layer, reassembles them into the original message, and passes the message to the correct application process.

### Internet Transport Protocols
The Internet offers two main transport protocols, which provide different service models:

1.  **TCP (Transmission Control Protocol):**
    - **Service Model:** Provides a **reliable, in-order** data transfer service. It is **connection-oriented**, meaning a handshake is required to set up a connection before data can be sent.
    - **Features:** Includes congestion control, flow control, and ensures all data arrives correctly and in the right order.
    - **Use Cases:** Applications that require high reliability, such as web browsing (HTTP), file transfer (FTP), and email (SMTP).

2.  **UDP (User Datagram Protocol):**
    - **Service Model:** Provides an **unreliable, "best-effort"** data transfer service. It is **connectionless**.
    - **Features:** A lightweight protocol with minimal overhead. It does not guarantee delivery, order, or integrity of data.
    - **Use Cases:** Applications that are sensitive to delay and can tolerate some data loss, such as real-time video streaming, online gaming, and DNS.

### Services Not Provided
The Internet's transport protocols do **not** provide certain guarantees that some applications might desire:
- **Delay Guarantees:** There is no guarantee on how long it will take for a segment to be delivered.
- **Bandwidth Guarantees:** There is no guarantee of a minimum throughput.

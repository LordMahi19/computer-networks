# Connection-Oriented Transport: TCP

**TCP (Transmission Control Protocol)** is the Internet's premier transport protocol, providing reliable, in-order delivery for the vast majority of web traffic, email, and file transfers.

### TCP Characteristics
- **Point-to-Point:** One sender, one receiver.
- **Reliable, In-order Byte Stream:** TCP presents the application with a continuous stream of bytes, abstracting away the underlying packets. It guarantees that all bytes will be delivered correctly and in the order they were sent.
- **Pipelining:** TCP is a pipelined protocol, allowing it to have multiple unacknowledged segments in flight, managed by its congestion and flow control mechanisms.
- **Full Duplex:** Data can flow in both directions simultaneously within the same connection.
- **Connection-Oriented:** A **three-way handshake** must be completed to establish a connection before any application data is exchanged.
- **Flow Controlled:** The receiver can control the sender's rate to prevent its own buffers from overflowing.

### TCP Segment Structure
A TCP segment consists of a header and a data payload. Key header fields include:
- **Source and Destination Port Numbers:** Used for multiplexing/demultiplexing.
- **Sequence Number:** A 32-bit number that indicates the byte-stream number of the *first byte* in the segment's data. This is crucial for ordering and acknowledging data.
- **Acknowledgement Number:** A 32-bit number that indicates the sequence number of the *next byte* the host is expecting from the other side. TCP uses **cumulative acknowledgements**. An ACK for sequence number 500 means all bytes up to 499 have been received correctly.
- **Receive Window (rwnd):** A 16-bit field used for flow control. It tells the other side how much buffer space is available at the receiver.
- **Header Length:** Specifies the length of the TCP header.
- **Flags (Control Bits):**
    - **SYN:** Used to initiate a connection.
    - **ACK:** Indicates that the acknowledgement number is valid.
    - **FIN:** Used to terminate a connection.
    - **RST:** Resets the connection.
    - **PSH:** Pushes the data to the application immediately.
    - **URG:** Urgent data pointer is valid.
- **Checksum:** Used for error detection.

### TCP Reliable Data Transfer
TCP uses the principles of RDT to ensure reliability:
- It uses **sequence numbers** to track bytes and **cumulative ACKs** to acknowledge them.
- It uses a single **retransmission timer** to handle lost segments.
- **Timeout Interval:** TCP dynamically calculates the timeout interval based on the measured **Round-Trip Time (RTT)** between the sender and receiver. It is set to be the `EstimatedRTT + 4 * DevRTT` (a safety margin based on the average RTT and its variance).
- **Fast Retransmit:** A crucial optimization. If a sender receives **three duplicate ACKs** for the same sequence number, it assumes the subsequent segment was lost and retransmits it immediately, without waiting for the timeout to expire. This significantly improves performance in the case of occasional packet loss.

### TCP Flow Control
- **Purpose:** To prevent the sender from overwhelming the receiver's buffer.
- **Mechanism:** The receiver advertises its available buffer space in the **receive window (rwnd)** field of every segment it sends. The sender ensures that the amount of unacknowledged ("in-flight") data is less than or equal to the advertised `rwnd`.

### TCP Connection Management

#### Three-Way Handshake (Establishing a Connection)
1.  **Client -> Server:** The client sends a TCP segment with the **SYN** flag set to 1. It also specifies its initial sequence number (`client_isn`).
2.  **Server -> Client:** The server receives the SYN, allocates buffers, and sends back a **SYNACK** segment. This segment has the SYN flag set to 1, the ACK field set to `client_isn + 1`, and specifies the server's own initial sequence number (`server_isn`).
3.  **Client -> Server:** The client receives the SYNACK, allocates buffers, and sends a final ACK segment. This segment has the ACK field set to `server_isn + 1`.

After this, the connection is established, and data can be exchanged.

#### Closing a Connection
1.  The client sends a segment with the **FIN** flag set to 1.
2.  The server receives the FIN and sends an ACK to acknowledge it. It then sends its own FIN segment.
3.  The client receives the server's FIN and sends a final ACK. After waiting for a period (to ensure the ACK is received), the connection is closed on both sides.

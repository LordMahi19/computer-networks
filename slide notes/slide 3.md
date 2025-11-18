Based on the provided excerpts from **"slide 3.pdf,"** the major categories and specific topics discussed relate to the **Transport Layer** of computer networks, focusing heavily on the foundational principles of transport protocols, particularly UDP and TCP.

Here are all the topics and categories mentioned in "slide 3.pdf":

### I. Transport Layer: Overview and Roadmap

- **Course Context:** Slides adopted from J.F Kurose and K.W. Ross's _Computer Networking: A Top-Down Approach_ 8th edition.
- **Our Goal:** Understand principles behind transport layer services.
- **Key Services Studied:**
    - **Multiplexing** and **demultiplexing**.
    - **Reliable data transfer** (RDT).
    - **Flow control**.
    - **Congestion control**.
- **Internet Transport Protocols Studied:**
    - **UDP:** Connectionless transport.
    - **TCP:** Connection-oriented reliable transport.
- **Evolution of transport-layer functionality**.

### II. Transport Layer Services and Core Functions

- **Logical Communication:** Provides logical communication between application processes running on different hosts.
- **Transport Protocols Actions in End Systems:** Sender breaks application messages into **segments** and passes them to the network layer; receiver reassembles segments into messages and passes them to the application layer.
- **Layer Comparison:** Transport layer provides logical communication between _processes_, relying on and enhancing network layer services (logical communication between _hosts_).
- **Services Not Available:** The Internet protocols do not provide **delay guarantees** or **bandwidth guarantees**.

### III. Multiplexing and Demultiplexing

- **Multiplexing (Sender):** Handling data from multiple sockets and adding a transport header.
- **Demultiplexing (Receiver):** Using header information (IP addresses and port numbers) to deliver received segments to the correct socket.
- **Connectionless Demultiplexing (UDP):** Uses the destination port number to direct the segment to the appropriate socket; segments with the same destination port but different source IPs/ports are directed to the same socket.
- **Connection-oriented Demultiplexing (TCP):** A TCP socket is uniquely identified by a **4-tuple** (source IP address, source port number, destination IP address, destination port number). All four values are used to direct the segment to the appropriate socket.

### IV. Connectionless Transport: UDP

- **Characteristics:** "No frills," "bare bones" protocol providing "best effort" service.
- **Service Model:** Unreliable, unordered delivery; segments may be lost or delivered out-of-order.
- **Advantages:** No connection establishment (no RTT delay), simple (no connection state), small header size, and no congestion control (can "blast away as fast as desired").
- **Use Cases:** Streaming multimedia apps (loss tolerant, rate sensitive), **DNS**, **SNMP**, and **HTTP/3**.
- **UDP Segment Header:** Contains source port #, destination port #, length (of segment, including header), and **checksum**.
- **UDP Checksum:** Used to detect errors (flipped bits) in the transmitted segment. Calculated as the one's complement sum of the segment contents (including header and IP addresses). It provides weak protection against errors.

### V. Principles of Reliable Data Transfer (RDT)

- **Abstraction:** RDT provides a reliable service abstraction over an unreliable channel.
- **RDT Protocol Development:** Progressively developed by considering underlying channel assumptions.
    - **RDT 1.0:** Reliable transfer over a perfectly reliable channel (no errors, no loss).
    - **RDT 2.0:** Channel with bit errors. Introduces **ACKs** (acknowledgements) and **NAKs** (negative acknowledgements) to recover from errors, and the **stop-and-wait** mechanism.
    - **RDT 2.1:** Handles garbled ACK/NAKs by using **sequence numbers** (0, 1 suffices) and retransmitting upon corrupted ACK/NAK receipt to handle potential duplicates.
    - **RDT 2.2:** A NAK-free protocol using cumulative ACKs (TCP uses this approach).
    - **RDT 3.0:** Channels with errors and **loss**. Uses a **countdown timer** (timeout) to trigger retransmission if no ACK is received within a reasonable time, which handles lost packets and ACKs.
- **Performance (Stop-and-Wait):** RDT 3.0 performance is poor due to low sender utilization, demonstrating that the protocol limits the performance of the underlying channel (e.g., utilization calculated as 0.00027 in one example).
- **Pipelining:** Allows the sender to have multiple, unacknowledged packets "in-flight," significantly increasing utilization. Requires increasing the sequence number range and buffering at the sender/receiver.
    - **Go-Back-N (GBN):** Sender maintains a window of up to $N$ unACKed packets. Receiver uses **cumulative ACKs** (ACK(n) ACKs all up to $n$). Timeout retransmits the timed-out packet and **all** subsequent packets in the window.
    - **Selective Repeat (SR):** Receiver individually acknowledges all correctly received packets and buffers out-of-order packets. Sender maintains a timer for **each** unACKed packet and retransmits individually on timeout. SR faces a dilemma regarding window size and sequence number space to avoid confusing old and new packets.

### VI. Connection-Oriented Transport: TCP

- **Key Characteristics:** **Point-to-point**, **reliable, in-order byte stream** (no message boundaries), **full duplex data**, **pipelining** (window size set by flow/congestion control), **flow controlled**, **cumulative ACKs**, and **connection-oriented** (connection setup required).
- **Maximum Segment Size (MSS):** Maximum segment size.
- **Segment Structure:** Includes source/dest port #, **sequence number**, **acknowledgement number**, **receive window (rwnd)**, checksum, and control flags (**ACK, RST, SYN, FIN**).
    - **Sequence Number:** Byte stream number of the first data byte in the segment.
    - **Acknowledgement Number:** Sequence number of the **next expected byte** from the other side (**cumulative ACK**).
    - **rwnd (Receive Window):** Number of bytes the receiver is willing to accept (**flow control**).
- **RTT and Timeout Estimation:**
    - **SampleRTT:** Measured time from segment transmission until ACK receipt (retransmissions ignored).
    - **EstimatedRTT:** Calculated using Exponential Weighted Moving Average (EWMA): $EstimatedRTT = (1- \alpha)_EstimatedRTT + \alpha_SampleRTT$ ($\alpha = 0.125$).
    - **TimeoutInterval:** Set with a safety margin based on the EstimatedRTT and its deviation ($DevRTT$): $TimeoutInterval = EstimatedRTT + 4*DevRTT$.
- **Sender (Simplified):** Starts a timer upon sending a segment (if not running). Timeout triggers retransmission of the segment that caused the timeout and timer restart. ACK receipt updates the boundary of acknowledged data and restarts the timer if unACKed segments remain.
- **Receiver (ACK Generation):** Specifies rules for immediate vs. delayed ACKs and sending **duplicate ACKs** (indicating a gap or out-of-order segment).
- **Fast Retransmit:** If the sender receives **three additional ACKs for the same data** ("triple duplicate ACKs"), it resends the unACKed segment with the smallest sequence number immediately, without waiting for the timeout.
- **Flow Control:** The receiver controls the sender's transmission rate using the **rwnd** field in the TCP header, guaranteeing that the receive buffer will not overflow.
- **Connection Management:**
    - **Handshake Goal:** Agree to establish a connection and agree on parameters (e.g., starting sequence numbers).
    - **TCP 3-Way Handshake:** Solves issues associated with premature timeouts, message reordering, and retransmissions in a 2-way handshake.
        1. Client sends **SYN** message (Seq=x).
        2. Server receives SYN, sends **SYNACK** (Seq=y, ACKnum=x+1).
        3. Client sends **ACK** (ACKnum=y+1).
    - **Closing Connection:** Both client and server close their side by sending a TCP segment with the **FIN bit = 1**.

### VII. Principles of Congestion Control

- **Definition:** Too many sources sending too much data too fast for the network to handle.
- **Manifestations:** Long delays (queueing) and packet loss (buffer overflow).
- **Costs of Congestion:**
    - Large delays as arrival rate approaches capacity (Scenario 1).
    - Reduced throughput due to **wasted capacity** from necessary retransmissions (Scenario 2).
    - Wasted upstream transmission/buffering capacity for packets lost downstream (Scenario 3).
- **Approaches:**
    - **End-end Congestion Control:** Congestion is inferred from observed loss/delay (TCP approach).
    - **Network-assisted Congestion Control:** Routers provide explicit feedback (e.g., ECN, ATM, DECbit protocols).

### VIII. TCP Congestion Control Mechanisms

- **AIMD (Additive Increase, Multiplicative Decrease):** Senders increase the sending rate (congestion window, $cwnd$) linearly until loss (congestion) occurs, then decrease the rate.
    - Increase rate by 1 MSS (Maximum Segment Size) every RTT (**Additive Increase**).
    - Cut sending rate in half at each loss event (**Multiplicative Decrease**).
    - Multiplicative decrease cuts rate in half on triple duplicate ACK (TCP Reno) or cuts to 1 MSS on timeout (TCP Tahoe).
- **TCP Sending Rate:** $\text{Rate} \approx \frac{cwnd}{RTT} \text{ bytes/sec}$.
- **Slow Start:** When a connection begins, the rate increases **exponentially** (doubling $cwnd$ every RTT, or incrementing $cwnd$ for every ACK received) until the first loss event or $cwnd$ reaches $ssthresh$.
- **Congestion Avoidance:** When $cwnd$ reaches $ssthresh$ (set to $1/2$ of $cwnd$ before loss), the rate increases **linearly**.
- **Fast Recovery:** Occurs after a triple duplicate ACK; $ssthresh$ is set to $cwnd/2$, and $cwnd$ is set to $ssthresh + 3$.
- **TCP CUBIC:** A congestion control algorithm (default in Linux) that aims for higher throughput by ramping up faster after a loss but approaching the maximum previous window size ($W_{max}$) more slowly, based on a cubic function of time.
- **Delay-based Congestion Control (BBR):** Aims to keep the bottleneck link busy ("just full") while avoiding high delays, by inferring congestion based on measured RTT versus minimum RTT ($RTT_{min}$). BBR is deployed on Google's backbone.
- **Fairness:** TCP aims for fairness (if $K$ sessions share bandwidth $R$, each gets $R/K$). This relies on additive increase and multiplicative decrease properties. UDP applications are often not "fair" as they send at a constant rate and tolerate loss, bypassing congestion control.

### IX. Evolution of Transport-Layer Functionality

- **New TCP Flavors:** Developed for specific scenarios like long-delay links, wireless links, and data center networks.
- **QUIC (Quick UDP Internet Connections):** Moving transport-layer functions to the application layer on top of **UDP** (used by HTTP/3) to increase performance.
- **QUIC Features:** Implements reliability, congestion control, authentication, and encryption. It establishes state (including security state) in **one RTT**, compared to TCP/TLS which requires two serial handshakes.
- **QUIC Streams:** Multiple application-level streams are multiplexed over a single QUIC connection, providing per-stream reliable data transfer, security, and congestion control, and eliminating **Head-of-Line (HOL) blocking**.
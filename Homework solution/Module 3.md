# 1. How is UDP socket fully identified? What about TCP socket What is the difference between the full identification of both sockets? 

A **UDP socket** is identified by a **two-tuple** (destination IP address and destination port number). Consequently, all incoming UDP segments with the same destination port are directed to the **same socket**, regardless of their source.

A **TCP socket** is identified by a **four-tuple** (source IP, source port, destination IP, and destination port). This allows a server to support multiple simultaneous connections to the same port (like port 80) by directing segments with different source information to **different sockets**.

The primary **difference** is that TCP uses the source IP and port to distinguish between connections, whereas UDP relies solely on the destination information for demultiplexing.

# 2. Describe why an application developer might choose to run an application over UDP rather than TCP.

An application developer might choose **User Datagram Protocol (UDP)** over TCP for the following reasons:

- **No Connection Delay:** UDP is **connectionless**, meaning it requires no initial handshaking, which avoids the Round-Trip Time (RTT) delay needed to establish a connection,.
- **Finer Control Over Timing:** Because UDP lacks **congestion control**, rate-sensitive applications can "blast away" data at a constant speed without being throttled by the protocol during network overloads,.
- **Reduced Overhead:** UDP is simpler than TCP, featuring a **small header size** and requiring no maintained connection state at the sender or receiver.
- **Suitability for Loss-Tolerant Apps:** It is ideal for applications that can tolerate some data loss but require low delay, such as **streaming multimedia, interactive games, and DNS**,.
- **Custom Reliability:** Developers can choose to implement their own error and congestion control specifically at the **application layer** if the standard TCP mechanisms are unsuitable,.


# 3. Why is it that voice and video traffic is often sent over TCP rather than UDP in today’s Internet? (Hint: The answer we are looking for has nothing to do with TCP’s congestion-control mechanism.) 


Voice and video traffic frequently use TCP over UDP for the following reasons:

- **Middlebox Compatibility:** Many **firewalls** are configured to block UDP traffic by default while allowing HTTP (TCP) traffic. Additionally, TCP connections are more easily managed by **Network Address Translation (NAT)** devices found in home and corporate routers.
- **Standardized Streaming:** Modern video delivery relies on **DASH (Dynamic, Adaptive Streaming over HTTP)**, which operates at the application layer and inherits TCP as its underlying transport protocol.
- **Infrastructure Support:** Using HTTP/TCP allows content to leverage widespread **web caches and Content Distribution Networks (CDNs)**, which are optimized to deliver HTTP-based content to the network edge.
- **Sufficient Bandwidth:** While TCP lacks timing guarantees, modern Internet bandwidth is often sufficiently provisioned to make "best-effort" service **"good enough"** for real-time applications.


# 4. Is it possible for an application to enjoy reliable data transfer even when the application runs over UDP? If so, how? 

**Yes**, it is possible for an application to achieve **reliable data transfer** even when running over UDP.

This is accomplished by **implementing reliability and error control mechanisms at the application layer**,. Since UDP itself provides "no-frills," unreliable delivery, the application must handle tasks such as loss detection and retransmission itself,.

A prominent example of this is **HTTP/3**, which uses the **QUIC protocol**; QUIC runs on top of UDP but incorporates its own **application-level reliability** and congestion control,,.

# 5. Suppose that a Web server runs in Host C on port 80. Suppose this Web server uses persistent connections, and is currently receiving requests from two different Hosts, A and B. Are all of the requests being sent through the same socket at Host C? If they are being passed through different sockets, do both of the sockets have port 80? Discuss and explain.

The requests are passed through **different sockets** at Host C. In TCP, each socket is uniquely identified by a **4-tuple** consisting of the source IP address, source port number, destination IP address, and destination port number.

Because Host A and Host B have different IP addresses, their 4-tuples are distinct, leading the server to create a **new socket** to communicate with each particular client. Both of these sockets on Host C use **port 80** as their local port, but they remain unique because the source IP address and/or source port number in the 4-tuple will differ for each connection. This allows the server to maintain multiple simultaneous persistent connections by demultiplexing incoming segments to the correct socket based on all four values.

# 6. Consider a stop-and-wait data-transfer protocol that provides error checking and retransmissions but uses only negative acknowledgments. Assume that negative acknowledgements are never corrupt. Would such protocol work over a channel with bit errors? What about over a lossy channel with bit errors?  [P14]

Based on the sources, a stop-and-wait protocol using **only negative acknowledgments (NAKs)** would function as follows:

- **Over a channel with bit errors (no loss):** This protocol **would not work** for a standard stop-and-wait delivery. In this model, the sender transmits a packet and waits for a response. If the receiver only sends a NAK when an error is detected, a **successful transmission results in silence**. Without a positive acknowledgment (ACK), the sender has no signal to indicate the packet was received correctly and thus would not know when to proceed to the next packet.
- **Over a lossy channel with bit errors:** This protocol **would not work**. If a packet is lost entirely, the receiver never receives it and therefore cannot generate a NAK. In a lossy environment, the sender would **wait indefinitely** for a response that will never arrive, as there is no positive confirmation to distinguish between a successfully delivered packet and a lost one.

Reliable data transfer over lossy channels typically requires both ACKs and timers to handle cases where neither data nor feedback reaches the intended destination.


# 7. Consider a scenario in which Host A and Host B want to send messages to Host C. Hosts A and C are connected by a channel that can lose and corrupt (but not reorder) messages. Hosts B and C are connected by another channel (independent of the channel connecting A and C) with the same properties. The transport layer at Host C should alternate in delivering messages from A and B to the layer above (that is, it should first deliver the data from a packet from A, then the data from a packet from B, and so on). Design a stop-and-wait-like error-control protocol for reliably transferring packets from A and B to C, with alternating delivery at C as described above. Give FSM descriptions of A and C. (Hint: The FSM for B should be essentially the same as for A.) Also, give a description of the packet format(s) used.

Based on the sources provided, the design for this reliable transfer protocol uses the principles of **rdt3.0**, a stop-and-wait protocol that accounts for packet corruption and loss using checksums, sequence numbers, acknowledgments (ACKs), and timers.

### **Packet Formats**

The protocol requires two types of packets: **Data Packets** sent by Hosts A and B, and **ACK Packets** sent by Host C.

1. **Data Packet (from A or B to C):**
    - **Sequence Number (1 bit):** Used to detect duplicate packets caused by lost ACKs or premature timeouts. It alternates between 0 and 1.
    - **Checksum:** Used by the receiver to detect bit errors in the packet.
    - **Payload (Data):** The actual message from the application layer.
2. **ACK Packet (from C to A or B):**
    - **ACK Number (1 bit):** Indicates the sequence number of the packet being acknowledged.
    - **Checksum:** Used by the sender to ensure the ACK itself was not corrupted.

---

### **FSM for Host A (and Host B)**

Host A and Host B follow the standard **rdt3.0 sender** finite state machine (FSM). They are independent of each other and only care about their own reliable delivery to C.

- **State 1: Wait for call 0 from above**
    - **Event:** `rdt_send(data)` is called.
    - **Action:** Create packet `sndpkt` with sequence 0 and a checksum; send it via `udt_send(sndpkt)`; start a countdown timer. Move to State 2.
- **State 2: Wait for ACK 0**
    - **Event:** `rdt_rcv(rcvpkt)` && (`corrupt(rcvpkt)` || `isACK(rcvpkt, 1)`).
    - **Action:** Ignore (wait for timeout or correct ACK).
    - **Event:** `timeout`.
    - **Action:** Resend `sndpkt` (seq 0) and restart the timer.
    - **Event:** `rdt_rcv(rcvpkt)` && `notcorrupt(rcvpkt)` && `isACK(rcvpkt, 0)`.
    - **Action:** Stop the timer. Move to State 3.
- **State 3: Wait for call 1 from above**
    - **Event:** `rdt_send(data)` is called.
    - **Action:** Create packet `sndpkt` with sequence 1 and a checksum; send it via `udt_send(sndpkt)`; start the timer. Move to State 4.
- **State 4: Wait for ACK 1**
    - **Event:** `rdt_rcv(rcvpkt)` && (`corrupt(rcvpkt)` || `isACK(rcvpkt, 0)`).
    - **Action:** Ignore.
    - **Event:** `timeout`.
    - **Action:** Resend `sndpkt` (seq 1) and restart the timer.
    - **Event:** `rdt_rcv(rcvpkt)` && `notcorrupt(rcvpkt)` && `isACK(rcvpkt, 1)`.
    - **Action:** Stop the timer. Move to State 1.

---

### **FSM for Host C**

Host C must alternate deliveries (A, then B, then A...). Because it must also handle potential duplicates from the sender that is currently "waiting its turn," its FSM must be more complex than a standard receiver. Host C identifies the source based on which channel the packet arrived from.

- **State 1: Wait for 0 from A**
    - **Event:** `rdt_rcv(pkt)` from A && `notcorrupt(pkt)` && `has_seq0(pkt)`.
    - **Action:** `extract(pkt, data)`, `deliver_data(data)`, send `ACK0` to Host A. Move to State 2.
    - **Event:** `rdt_rcv(pkt)` from A && (`corrupt(pkt)` || `has_seq1(pkt)`).
    - **Action:** Send `ACK1` to Host A (acknowledging the last correctly received packet from A to stop A from retransmitting).
    - **Event:** `rdt_rcv(pkt)` from B.
    - **Action:** Since it is A's turn, C cannot deliver B's data yet. It sends an ACK for the last sequence number it successfully delivered from B to prevent Host B from timing out and retransmitting indefinitely.
- **State 2: Wait for 0 from B**
    - **Event:** `rdt_rcv(pkt)` from B && `notcorrupt(pkt)` && `has_seq0(pkt)`.
    - **Action:** `extract(pkt, data)`, `deliver_data(data)`, send `ACK0` to Host B. Move to State 3.
    - **Event:** `rdt_rcv(pkt)` from B && (`corrupt(pkt)` || `has_seq1(pkt)`).
    - **Action:** Send `ACK1` to Host B (acknowledging the last correctly received packet from B).
    - **Event:** `rdt_rcv(pkt)` from A.
    - **Action:** Send `ACK0` to Host A (the last sequence delivered from A).
- **State 3: Wait for 1 from A**
    - **Event:** `rdt_rcv(pkt)` from A && `notcorrupt(pkt)` && `has_seq1(pkt)`.
    - **Action:** `extract(pkt, data)`, `deliver_data(data)`, send `ACK1` to Host A. Move to State 4.
    - **Event:** (Similar to State 1, but expecting Seq 1 from A).
- **State 4: Wait for 1 from B**
    - **Event:** `rdt_rcv(pkt)` from B && `notcorrupt(pkt)` && `has_seq1(pkt)`.
    - **Action:** `extract(pkt, data)`, `deliver_data(data)`, send `ACK1` to Host B. Move to State 1.
    - **Event:** (Similar to State 2, but expecting Seq 1 from B).

This FSM ensures that Host C only delivers data to the upper layer in the strictly alternating order of A then B, while using sequence-numbered ACKs to keep both senders synchronized and to handle packet loss or corruption on their independent channels.

# 8. We have said that an application may choose UDP for a transport protocol because UDP offers finer application control (than TCP) of what data is sent in a segment and when.

a.       Why does an application have more control of what data is sent in a segment?

b.      Why does an application have more control on when the segment is sent? [P25]

Based on the sources, here is the explanation for why an application has finer control when using UDP:

**a. Control over what data is sent in a segment** UDP is a **"no-frills"** protocol that lacks the complex congestion and flow control mechanisms found in TCP. When an application uses UDP, the transport layer simply takes the application-layer message, determines the header fields, and **encapsulates it directly into a segment** to be passed to the network layer. Unlike TCP, which views data as a **byte stream** and may bundle or split data into segments based on its own algorithms, UDP allows the application to dictate exactly what data is contained within each individual segment.

**b. Control over when the segment is sent** UDP provides more control over timing because it does not implement **congestion control** or **flow control**, which are mechanisms in TCP that throttle or delay segments when the network is overloaded or the receiver is overwhelmed. Additionally, UDP is **connectionless**, meaning it requires **no handshaking** or connection setup before transmission, which avoids the Initial Round Trip Time (RTT) delay required by TCP. This allows an application to **"blast away"** and send data as fast as desired the moment it is ready.

# question 9

```

Consider transferring an enormous file of L bytes from Host A to Host B. Assume an MSS of 536 bytes.

a. What is the maximum value of L such that TCP sequence numbers are not exhausted? Recall that the TCP sequence number field has 4 bytes.

b. For the L you obtain in (a), find how long it takes to transmit the file. Assume that a total of 66 bytes of transport, network, and data-link header are added to each segment before the resulting packet is sent out over a 155 Mbps link. Ignore flow control and congestion control so A can pump out the segments back to back and continuously. 
```

**Given Information:**

- **File size ($L$):** $2^{32}$ bytes.
- **Maximum Segment Size ($MSS$):** 536 bytes.
- **TCP sequence number field:** 4 bytes (32 bits).
- **Total header size per segment:** 66 bytes (not mentioned in sources).
- **Link transmission rate ($R$):** 155 Mbps ($155 \times 10^6$ bps).

**a. Maximum value of $L$** The TCP sequence number is a 32-bit field in the segment header. These sequence numbers represent a count of the bytes in the data stream rather than a count of segments. A 32-bit field can represent $2^{32}$ unique values, which limits the total unique bytes in a stream before exhaustion. **$L_{max} = 2^{32}$ bytes = 4,294,967,296 bytes.**

**b. Transmission Time Calculation** The total number of segments required to transmit the file is determined by the file size divided by the MSS. $\text{Number of segments} = \lceil L / MSS \rceil = \lceil 4,294,967,296 / 536 \rceil = 8,012,999 \text{ segments}$. Each segment is encapsulated with transport, network, and data-link headers totalling 66 bytes. $\text{Total bytes per packet} = MSS + \text{Headers} = 536 \text{ bytes} + 66 \text{ bytes} = 602 \text{ bytes}$. The total number of bits to be transmitted is the product of the number of segments, the bytes per packet, and bits per byte. $\text{Total bits} = 8,012,999 \times 602 \times 8 = 38,590,603,184 \text{ bits}$. Transmission time is the total bits divided by the link transmission rate $R$. $\text{Time} = \frac{38,590,603,184 \text{ bits}}{155,000,000 \text{ bps}} = 248.97 \text{ seconds}$. **Total Transmission Time: 248.97 seconds.**


# 10. We know that TCP waits until it has received three duplicate ACKs before performing a fast retransmit (Section 3.5.4 page 275, in course book (8th edition)) . Why do you think the TCP designers chose not to perform a fast retransmit after the first duplicate ACK for a segment is received?

TCP designers wait for **three duplicate ACKs** rather than one to accurately distinguish between **packet reordering** and actual **packet loss**.

- **Evidence of Loss:** A single duplicate ACK often indicates a segment merely arrived out of order. Receiving three duplicate ACKs strongly suggests a loss because it implies three subsequent segments successfully reached the receiver while the expected one did not.
- **Avoiding Wasted Capacity:** Retransmitting after the first duplicate ACK would lead to **un-needed retransmissions**. These duplicates "waste" link capacity and decrease the maximum achievable throughput of the connection.
- **Efficiency:** This heuristic allows for a **fast retransmit** that is much quicker than waiting for a full **timeout**, yet reliable enough to avoid the costs of premature recovery.
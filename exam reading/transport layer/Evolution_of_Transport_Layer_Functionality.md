# Evolution of Transport-Layer Functionality

The transport layer is not static. New protocols and approaches are continually being developed to improve performance and address the limitations of traditional TCP.

### New TCP Flavors
Over the years, many different "flavors" of TCP have been developed to optimize performance in specific environments:
- **TCP Reno:** The classic version that introduced Fast Recovery.
- **TCP Tahoe:** An earlier version that always entered Slow Start after a loss.
- **TCP Vegas:** An early attempt at delay-based congestion control.
- **TCP CUBIC:** The current default in many operating systems, designed for high-bandwidth, high-latency networks ("long, fat pipes").
- **BBR (Bottleneck Bandwidth and Round-trip propagation time):** A modern, delay-based approach from Google that aims to maximize throughput while keeping queuing delays low.

### QUIC (Quick UDP Internet Connections)
QUIC is a revolutionary new transport protocol developed by Google, now standardized by the IETF. It is the foundation for **HTTP/3**.

- **Built on UDP:** QUIC is implemented in the application layer and runs on top of **UDP**. This allows it to be deployed and updated much more quickly than TCP, which is typically implemented deep within the operating system kernel.

- **Key Features and Advantages over TCP:**
    1.  **Reduced Connection Establishment Latency:** A new QUIC connection can be established in **1 RTT**. If a client has communicated with a server before, it can be **0-RTT**. This is a significant improvement over TCP+TLS, which requires 2-3 RTTs.
    2.  **Elimination of Head-of-Line (HOL) Blocking:**
        - In TCP, if one packet is lost, all subsequent packets (even if they have been received correctly) must wait for the lost packet to be retransmitted. This is HOL blocking.
        - QUIC supports multiple independent, parallel **streams** over a single connection. If a packet for one stream is lost, it only blocks that specific stream; other streams can continue to deliver data. This is a major advantage for HTTP/2-style web traffic with many small objects.
    3.  **Improved Congestion Control:** QUIC's congestion control is more precise and extensible than TCP's.
    4.  **Built-in Security:** QUIC connections are always authenticated and encrypted (similar to TLS).

QUIC represents a major shift, moving transport-layer functionality out of the OS kernel and into the application space, enabling faster innovation and better performance for the modern web.

# Network Performance Metrics

Understanding network performance is crucial for designing and managing networks. The key metrics are ==delay (latency), packet loss, and throughput.==

### 1. Packet Delay (Latency)
A packet experiences several types of delay as it travels from a source to a destination. The total delay at a single router (nodal delay) is the sum of four components:

- **Processing Delay ($d_{\text{proc}}$):**
    - The time it takes a router to process a packet's header, check for bit errors, and determine the output link.
    - Typically very small (microseconds).

- **Queuing Delay ($d_{\text{queue}}$):**
    - The time a packet spends waiting in a queue (buffer) before it can be transmitted on the output link.
    - This is the most variable type of delay and depends heavily on the network's congestion level.
    - If the **traffic intensity** (rate of arriving bits / link transmission rate) is close to 1, queuing delay can become very large. If it exceeds 1, the queue will grow indefinitely, and in practice, packets will be dropped.

- **Transmission Delay ($d_{\text{trans}}$):**
    - The time required to push all of the packet's bits onto the link.
    - Formula: **$d_{\text{trans}} = L / R$**
        - $L$ = Packet length (bits)
        - $R$ = Link transmission rate (bandwidth, in bits per second)

- **Propagation Delay ($d_{\text{prop}}$):**
    - The time it takes for a bit to travel from the beginning to the end of the physical link.
    - Formula: **$d_{\text{prop}} = d / s$**
        - $d$ = Length of the physical link (meters)
        - $s$ = Propagation speed of the medium (typically ~2x10⁸ m/s for copper/fiber)

**Total Nodal Delay:** $d_{\text{nodal}} = d_{\text{proc}} + d_{\text{queue}} + d_{\text{trans}} + d_{\text{prop}}$

### 2. Packet Loss
- **Cause:** Packet loss occurs when a packet arrives at a router and finds that the corresponding queue (buffer) is full. The router has no choice but to **drop** the packet.
- **Consequences:** The lost packet may be retransmitted by the source or a previous node, or it may not be retransmitted at all, depending on the protocol (e.g., TCP retransmits, UDP does not). Retransmissions consume extra bandwidth and can reduce overall throughput.

### 3. Throughput
- **Definition:** The rate (in bits per second) at which bits are actually being transferred between a sender and receiver.
- **Instantaneous vs. Average:** Throughput can be measured at a specific point in time (instantaneous) or over a longer period (average).
- **Bottleneck Link:** In a path consisting of multiple links, the end-to-end throughput is constrained by the link with the *lowest* transmission rate. This is known as the **bottleneck link**.
    - **Example:** If you have a path with two links, one at 100 Mbps and one at 10 Mbps, the maximum achievable throughput for that path will be 10 Mbps.

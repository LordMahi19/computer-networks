# Principles of Congestion Control

**Congestion** is a state where the network is overloaded: too many sources are sending too much data too fast for the network's routers and links to handle.

### Manifestations of Congestion
- **Long Delays:** As packet arrival rates approach link capacity, queues in routers build up, leading to long queuing delays.
- **Packet Loss:** When router buffers overflow, the router has no choice but to drop packets.

### Costs of Congestion

1.  **Reduced Throughput due to Retransmissions:** When a packet is lost, the sender must retransmit it. This retransmission consumes bandwidth that could have been used to send a new packet. This is a form of **wasted capacity**.

2.  **Wasted Upstream Capacity:** When a packet is dropped at a router deep inside the network, all the transmission capacity it consumed on the upstream links leading to that router is wasted.

3.  **Very High Delays:** As queues grow, the round-trip time (RTT) for packets increases significantly, harming the performance of interactive applications.

### Approaches to Congestion Control

There are two main approaches to how a sender can become aware of and react to congestion:

1.  **End-to-End Congestion Control:**
    - In this approach, the network provides no explicit support. The end systems must infer congestion from network behavior.
    - **How it works:** The sender observes events like packet loss (indicated by timeouts or duplicate ACKs) and increasing round-trip times as signs of congestion.
    - **Example:** This is the approach taken by **TCP**.

2.  **Network-Assisted Congestion Control:**
    - In this approach, network routers provide explicit feedback to the sender about the state of congestion.
    - **How it works:**
        - A router can send a "choke" packet directly to the sender.
        - A router can mark a field in a packet (e.g., using **Explicit Congestion Notification (ECN)**) to signal congestion. The receiver then informs the sender in its next acknowledgement.
    - **Examples:** ATM ABR, DECbit.

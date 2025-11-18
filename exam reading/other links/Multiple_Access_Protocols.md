# Multiple Access Protocols

When multiple nodes are connected to a shared broadcast channel (e.g., WiFi, old-fashioned Ethernet, satellite link), a protocol is needed to coordinate their transmissions and handle collisions. This is the job of a **Multiple Access Control (MAC)** protocol.

**The Problem:** If two or more nodes transmit at the same time, their signals will interfere with each other, resulting in a **collision**. The receiving nodes will not be able to correctly interpret any of the transmitted frames.

An ideal MAC protocol should be:
- **Efficient:** If one node has data to send, it should be able to use the full rate of the channel ($R$ bps).
- **Fair:** If $M$ nodes have data to send, each should get an average share of the bandwidth ($R/M$).
- **Decentralized and Simple:** To avoid single points of failure and keep costs low.

There are three main classes of MAC protocols:

### 1. Channel Partitioning Protocols
These protocols divide the channel's resources among the nodes, giving each node an exclusive piece. This completely avoids collisions.

- **Time Division Multiple Access (TDMA):**
    - Time is divided into rounds, and each round is divided into a fixed number of time slots.
    - Each node is assigned a specific time slot in each round.
    - **Drawback:** A node can only transmit during its assigned slot, even if it's the only node with data to send. Unused slots go idle, wasting bandwidth.

- **Frequency Division Multiple Access (FDMA):**
    - The channel's frequency spectrum is divided into different frequency bands.
    - Each node is assigned a specific frequency band.
    - **Drawback:** Similar to TDMA, the bandwidth is wasted if a node has no data to send.

### 2. Random Access Protocols
In these protocols, there is no pre-coordination. Each node transmits at the full channel rate. Collisions can happen, and the protocol specifies how to detect them and recover.

- **Slotted ALOHA:**
    - Time is divided into slots.
    - When a node has a frame to send, it waits for the beginning of the next slot and transmits.
    - If a collision occurs, the node detects it (e.g., by not receiving an ACK) and retransmits the frame in a subsequent slot with some probability $p$.
    - **Maximum efficiency:** 37%.

- **Pure (Unslotted) ALOHA:**
    - Simpler than Slotted ALOHA. Nodes transmit immediately when they have a frame.
    - Collisions are more likely because transmissions are not synchronized to slots.
    - **Maximum efficiency:** 18%.

- **CSMA (Carrier Sense Multiple Access):**
    - **"Listen before you speak."** A node first senses the channel to see if another node is transmitting.
    - If the channel is idle, it transmits. If it's busy, it defers its transmission.
    - **Problem:** Collisions can still occur due to propagation delay. Two nodes at opposite ends of a link might both sense the channel as idle and start transmitting at nearly the same time.

- **CSMA/CD (CSMA with Collision Detection):**
    - **"Listen while you speak."** An enhancement to CSMA. While a node is transmitting, it also listens to the channel.
    - If it detects that its signal is interfering with another signal, it knows a collision has occurred.
    - It immediately aborts its transmission, sends a jam signal to alert other nodes, and then waits a random amount of time before trying again (this is called **binary backoff**).
    - **This is the MAC protocol used by classic Ethernet.**

### 3. "Taking Turns" Protocols
These protocols attempt to combine the best features of channel partitioning (good at high load) and random access (good at low load).

- **Polling:**
    - A "master" node invites "slave" nodes to transmit in turn.
    - **Drawbacks:** Introduces polling delay, overhead, and a single point of failure (the master).

- **Token Passing:**
    - A special frame called a **token** is passed from node to node in a logical ring.
    - A node can only transmit if it is holding the token.
    - **Drawbacks:** Overhead of passing the token, latency, and a single point of failure (if the token is lost).
    - Used in Token Ring and FDDI networks.

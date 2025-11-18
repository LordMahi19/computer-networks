# The Network Core

The network core is the mesh of interconnected routers that connects the Internet's end systems. Its primary function is to move data between hosts. This is achieved through two main approaches: packet switching and circuit switching.

### Packet Switching
This is the dominant method used in the Internet.

- **Core Idea:** Application messages are broken into smaller chunks called **packets**. These packets are sent from the source, through a series of routers, to the destination, where they are reassembled.
- **Store-and-Forward:** Each router must receive the *entire* packet before it can begin transmitting it on the next link. This introduces a transmission delay at each hop.
- **Queuing and Loss:**
    - If a router's output link is busy when a packet arrives, the packet is placed in a **queue** (or buffer).
    - This queuing introduces delay.
    - If the queue is full, the router will **drop** (lose) the arriving packet.
- **Key Functions:**
    - **Forwarding (Data Plane):** The local action of a router moving a packet from an input link to the appropriate output link.
    - **Routing (Control Plane):** The global action of determining the end-to-end path that packets take from source to destination. Routing algorithms create forwarding tables that are used in the forwarding process.

### Circuit Switching
This was the traditional method used in telephone networks.

- **Core Idea:** End-to-end resources (buffers, link transmission rate) are **reserved** for the duration of a "call" or session.
- **Dedicated Resources:** The reserved resources are guaranteed for the session, providing predictable performance (no queuing delays after setup).
- **Idle Resources:** If the session is not actively sending data, the reserved circuit is idle and its capacity is wasted.
- **Implementation:**
    - **Frequency Division Multiplexing (FDM):** The frequency spectrum of a link is divided among the connections.
    - **Time Division Multiplexing (TDM):** Time is divided into frames, and each frame is divided into a fixed number of time slots. Each connection gets one time slot per frame.

### Packet Switching vs. Circuit Switching

| Feature             | Packet Switching                                       | Circuit Switching                                      |
| ------------------- | ------------------------------------------------------ | ------------------------------------------------------ |
| **Resource Allocation** | On-demand, resources are not reserved.                 | Resources are reserved for the entire session.         |
| **Performance**     | Prone to congestion, delay, and loss.                  | Guaranteed, predictable performance (after setup).     |
| **Efficiency**      | More efficient for "bursty" data (like web traffic).   | Inefficient if reserved circuits are idle.             |
| **Complexity**      | Simpler, no call setup required.                       | Requires complex call setup and signaling.             |

Packet switching is better suited for data traffic, which is often bursty and can tolerate some delay, because it allows for more efficient sharing of network resources.

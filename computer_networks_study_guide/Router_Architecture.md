# Router Architecture

A router is a network-layer device whose primary role is to forward datagrams from input links to output links. High-performance routers are complex systems designed to process packets at extremely high speeds.

A router has two main functional planes:
- **Data Plane (Forwarding Plane):** The local, per-router function of moving a datagram from an input port to an output port. This is typically implemented in hardware for speed.
- **Control Plane (Routing Plane):** The network-wide logic that determines the paths datagrams take. This involves running routing protocols to create the forwarding tables. This is typically implemented in software by a routing processor.

### High-Level Router Architecture

A router consists of four main components:

1.  **Input Ports:**
    - **Physical Layer:** Terminates the physical link.
    - **Link Layer:** Implements the link-layer protocol (e.g., Ethernet).
    - **Network Layer (Lookup & Forwarding):** This is the most critical function. The router uses the datagram's destination IP address to look up the appropriate output port in its **forwarding table**. This lookup must be performed at "line speed".
    - **Queuing:** If datagrams arrive faster than they can be forwarded through the switching fabric, they are queued at the input port.

2.  **Switching Fabric:**
    - The "heart" of the router that connects the input ports to the output ports.
    - Its job is to move packets from input queues to the correct output queues.
    - **Types of Switching Fabrics:**
        - **Switching via Memory (First Generation):** Packets are copied from the input port to the main system memory, and then from memory to the output port. This is slow and limited by memory bandwidth.
        - **Switching via a Bus:** An input port transfers a packet directly to the output port over a shared bus. This is faster but limited by the bus bandwidth.
        - **Switching via Interconnection Network (Crossbar):** A sophisticated network of busses and interconnects that allows multiple packets to be forwarded in parallel. This is the approach used in high-performance routers.

3.  **Output Ports:**
    - **Queuing and Buffering:** The output port stores packets that have been forwarded to it from the switching fabric. Queuing occurs if the packets arrive from the fabric faster than the output link can transmit them.
    - **Buffer Management:** If the output buffer fills up, the router must decide which packets to drop. This is handled by a **drop policy** (e.g., tail drop, priority drop).
    - **Packet Scheduling:** The scheduler decides which packet to send next from the queue. Common algorithms include **First-Come, First-Served (FCFS)**, **Priority Scheduling**, and **Weighted Fair Queuing (WFQ)**.
    - **Link and Physical Layer:** Implements the link and physical layer protocols for the outgoing link.

4.  **Routing Processor (Control Plane):**
    - Executes the routing protocols (e.g., OSPF, BGP).
    - Maintains the routing information and computes the forwarding table.
    - In traditional routers, the routing processor is responsible for populating the forwarding table used by the data plane.
    - In **Software-Defined Networking (SDN)**, the routing processor communicates with a remote controller, which computes and installs the forwarding table.

### Bottlenecks and Blocking
- **Input Port Queuing:** If the switching fabric is not fast enough to keep up with the combined rate of all input ports, queuing can occur at the input ports.
- **Head-of-the-Line (HOL) Blocking:** At an input port, a queued datagram at the front of the queue can prevent other datagrams behind it from moving forward, even if their output ports are free.
- **Output Port Queuing:** This is the most common bottleneck. It occurs when the rate of packets arriving at the output port (from the fabric) exceeds the transmission rate of the output link. This is why buffer management and scheduling at the output port are so important.

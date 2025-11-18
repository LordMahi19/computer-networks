# Ethernet

Ethernet is, by far, the dominant wired LAN technology. It has evolved significantly over the years, from a shared bus to a high-speed, switched network.

### Ethernet Topologies
- **Bus Topology (Classic Ethernet):**
    - In the original design, all nodes were connected to a single coaxial cable (a "bus").
    - This created a **single collision domain**: if two nodes transmitted at the same time, the signals would collide on the bus.
    - The MAC protocol used was **CSMA/CD** to manage these collisions.

- **Star Topology (Modern Ethernet):**
    - Today, all nodes are connected to a central device called a **switch**.
    - Each node has a dedicated, point-to-point link to the switch.
    - This means there are **no collisions** on the links between hosts and the switch. Each link is its own separate collision domain.
    - Modern Ethernet is typically **full-duplex**, meaning a node can send and receive data at the same time.

### Ethernet Frame Structure
The Ethernet frame specifies the format for data sent over an Ethernet link.

| Field                 | Size      | Description                                                                                             |
| --------------------- | --------- | ------------------------------------------------------------------------------------------------------- |
| **Preamble**          | 8 bytes   | A sequence of alternating 1s and 0s used by the receiver's NIC to synchronize its clock with the sender's. |
| **Destination Address** | 6 bytes   | The MAC address of the destination interface.                                                           |
| **Source Address**    | 6 bytes   | The MAC address of the sending interface.                                                               |
| **Type**              | 2 bytes   | Indicates the higher-layer protocol encapsulated in the frame (e.g., `0x0800` for IP, `0x0806` for ARP). This is used for demultiplexing at the receiver. |
| **Data (Payload)**    | 46-1500 bytes | The encapsulated datagram from the network layer (e.g., an IP datagram).                                |
| **CRC**               | 4 bytes   | A 32-bit Cyclic Redundancy Check used for error detection. The receiver calculates the CRC on the received frame and drops the frame if it doesn't match. |

### Ethernet Service Model
- **Connectionless:** No handshake is performed between sending and receiving NICs.
- **Unreliable:** The receiving NIC does not send acknowledgements. If the CRC check fails and a frame is dropped, it is up to a higher-layer protocol (like TCP) to detect this loss and recover.

### Ethernet Speeds
Ethernet has evolved through many different speed standards, including:
- 10 Mbps (Classic Ethernet)
- 100 Mbps (Fast Ethernet)
- 1 Gbps (Gigabit Ethernet)
- 10 Gbps, 40 Gbps, 100 Gbps, and even 400 Gbps.
These standards use a variety of different physical media, including coaxial cable, twisted-pair copper wire, and fiber optic cable.

# Link-Layer Switches

A **switch** is a link-layer device that is central to modern Ethernet LANs. Its job is to receive incoming frames and forward them to the correct outgoing link.

### Switch Characteristics
- **Store-and-Forward:** A switch buffers frames as they arrive and forwards them one at a time.
- **Transparent:** Switches are "plug-and-play" devices that are transparent to the hosts on the LAN. Hosts are not aware that they are connected to a switch; they behave as if they are on a shared Ethernet segment.
- **Self-Learning:** Switches automatically learn the locations of hosts on the network without any manual configuration.

### How a Switch Works: Forwarding and Filtering
A switch maintains a **switch table** (also called a MAC table or forwarding table). This table contains entries that map the MAC addresses of hosts to the switch port (interface) that they are connected to.

**Switch Table Entry:** `< MAC Address, Switch Port, TTL >`

The switch performs three key actions:

1.  **Examine and Learn (Self-Learning):**
    - For every frame that arrives at the switch, the switch inspects the **source MAC address**.
    - It records this source MAC address and the port it arrived on in the switch table. If an entry for that address already exists, it updates the TTL.
    - This is how the switch automatically learns which hosts are connected to which of its ports.

2.  **Forward or Filter:**
    - The switch inspects the **destination MAC address** of the frame.
    - It looks up this destination MAC address in its switch table.

    - **Case 1: Entry Found (Address is Known)**
        - If the destination port is the *same* as the port the frame arrived on, it means the destination is on the same segment as the source. The switch **filters** (drops) the frame, as there is no need to forward it.
        - If the destination port is *different* from the arriving port, the switch **forwards** the frame only to that specific destination port. This is a key benefit of switches: they create dedicated connections and prevent traffic from being sent unnecessarily to other nodes.

    - **Case 2: No Entry Found (Address is Unknown)**
        - If the switch has no entry for the destination MAC address, it does not know where to send the frame.
        - In this case, the switch **floods** the frame—it forwards a copy of the frame out of *all* of its ports, except for the one it arrived on.
        - The correct destination host will eventually receive the frame. When that host replies, the switch will learn its location from the source address of the reply frame, and future frames to that host can be forwarded directly.

### Switches vs. Routers

| Feature            | Switch                                                     | Router                                                     |
| ------------------ | ---------------------------------------------------------- | ---------------------------------------------------------- |
| **Layer**          | Operates at the **Link Layer** (Layer 2).                  | Operates at the **Network Layer** (Layer 3).               |
| **Address Used**   | Forwards frames based on **MAC addresses**.                | Forwards datagrams based on **IP addresses**.              |
| **Table Building** | Learns its forwarding table **automatically** (self-learning). | Computes its forwarding table using **routing algorithms** (e.g., OSPF, BGP) in the control plane. |
| **Domain**         | Connects hosts within the same **broadcast domain** (LAN). | Connects different **subnets** (broadcast domains).        |
| **Scalability**    | Do not offer hierarchical addressing; broadcast traffic can be a bottleneck. | Provide hierarchical addressing and do not forward broadcast traffic, making them more scalable. |

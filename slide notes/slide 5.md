The sources extracted from **"slide 5.pdf"** focus entirely on the **Link Layer and Local Area Networks (LANs)**, detailing the underlying services, mechanisms for sharing a medium, addressing, and specific technologies like Ethernet and VLANs.

Here are all the topics and categories mentioned in "slide 5.pdf":

### I. Link Layer and LANs: Overview and Goals

- **Goal:** Understand principles behind link layer services.
- **Key Services Studied:**
    - **Error detection, correction**.
    - **Sharing a broadcast channel: multiple access**.
    - **Link layer addressing**.
    - **Local area networks (LANs):** Ethernet, VLANs.
- **Implementation Focus:** Instantiation and implementation of various link layer technologies.
- **Roadmap:** Introduction; error detection, correction; multiple access protocols; LANs (addressing, ARP, Ethernet, switches, VLANs).

### II. Link Layer Introduction and Services

- **Terminology:**
    - **Nodes:** Hosts and routers.
    - **Links:** Communication channels connecting adjacent nodes (wired, wireless, LANs).
    - **Layer-2 packet:** Frame, which encapsulates a datagram.
- **Core Responsibility:** Transferring a datagram from one node to a physically adjacent node over a link.
- **Context/Analogy:** Datagram transferred by different link protocols over different links (e.g., WiFi then Ethernet). The trip analogy compares the datagram to a tourist, the link segment to a transportation segment, and the link-layer protocol to the transportation mode.
- **Link Layer Services:**
    - **Framing, Link Access:** Encapsulating a datagram into a frame (adding header/trailer), and channel access if a shared medium is used. **MAC addresses** (different from IP addresses) identify source and destination.
    - **Reliable Delivery:** Between adjacent nodes. Seldom used on low bit-error links but important for high error rate wireless links.
    - **Flow Control:** Pacing between adjacent sending and receiving nodes.
    - **Error Detection:** Receiver detects errors (caused by signal attenuation/noise) and signals retransmission or drops the frame.
    - **Error Correction:** Receiver identifies and corrects bit error(s) without retransmission.
    - **Duplexing:** **Half-duplex** (nodes transmit, but not at the same time) and **full-duplex**.
- **Implementation:** Implemented in the **Network Interface Card (NIC)** or on a chip (e.g., Ethernet, WiFi card or chip). It is a combination of hardware, software, and firmware.

### III. Error Detection and Correction

- **Error Detection Concepts:** Involves **EDC** (error detection and correction bits) or redundancy. Error detection is not 100% reliable, but a larger EDC field yields better results.
- **Parity Checking:**
    - **Single Bit Parity:** Detects single bit errors.
    - **Two-Dimensional Bit Parity:** Detects and corrects single bit errors.
- **Cyclic Redundancy Check (CRC):**
    - A more powerful error-detection coding scheme.
    - Goal: Choose $r$ CRC bits ($R$) such that the frame $<D,R>$ (data $D$ XOR $R$) is exactly divisible by a given bit pattern $G$ (Generator) modulo 2.
    - The receiver detects an error if the remainder is non-zero after dividing the received frame by $G$.
    - Can detect all **burst errors** less than $r+1$ bits.
    - Widely used in practice (e.g., Ethernet, 802.11 WiFi).

### IV. Multiple Access Protocols (MAC)

- **Link Types:** **Point-to-point** (e.g., Ethernet switch to host, PPP) and **broadcast** (shared wire or medium). Broadcast examples include old-fashioned Ethernet, upstream HFC, 802.11 wireless LAN, 4G/5G, satellite, and humans at a cocktail party.
- **Problem:** Interference/collision occurs with two or more simultaneous transmissions.
- **MAC Protocol Goal:** A distributed algorithm determining how nodes share the channel. Communication about sharing must use the channel itself.
- **Ideal Protocol Desiderata:** Send at rate $R$ when one node transmits, or average rate $R/M$ when $M$ nodes transmit; fully decentralized; simple.
- **MAC Protocol Taxonomy (Three Classes):**
    1. **Channel Partitioning:** Divide channel into pieces (time slots, frequency, code) and allocate exclusive use to nodes.
        - **TDMA (Time Division Multiple Access):** Fixed length slots in rounds; unused slots go idle.
        - **FDMA (Frequency Division Multiple Access):** Channel spectrum divided into fixed frequency bands; unused time in bands goes idle.
    2. **Random Access:** Channel not divided; collisions allowed, and protocol specifies how to detect and recover via delayed retransmissions.
        - **ALOHA, slotted ALOHA:** Nodes transmit in the next slot. Slotted ALOHA has a maximum theoretical efficiency of $1/e \approx 0.37$. **Pure ALOHA** (unslotted) has lower efficiency (18%).
        - **CSMA (Carrier Sense Multiple Access):** "Listen before transmit". Collisions can still occur due to propagation delay.
        - **CSMA/CD (CSMA with Collision Detection):** Collisions detected and aborted quickly, reducing channel wastage. Used in Ethernet.
    3. **"Taking Turns" Protocols:** Nodes take turns, balancing efficiency at high load (like partitioning) with low delay at low load (like random access).
        - **Polling:** Master node invites slaves to transmit. Concerns: overhead, latency, single point of failure.
        - **Token Passing:** Control token message passed sequentially. Concerns: overhead, latency, single point of failure. Used in Bluetooth, FDDI, and token ring.
- **Cable Access Networks (DOCSIS):** Use FDM over channels, TDM upstream (for assigned slots), and **random access** (binary backoff) for requesting upstream channel time slots (contention).

### V. LANs: Addressing, ARP, and Ethernet

- **MAC Addresses:**
    - Also called LAN, physical, or Ethernet addresses.
    - **48-bit addresses**, typically burned into NIC ROM.
    - Used **locally** to get a frame between two physically-connected interfaces on the same subnet.
    - Administered by IEEE, ensuring uniqueness by manufacturer.
    - **Flat address:** Provides portability (can move NIC to a different LAN).
- **ARP (Address Resolution Protocol):**
    - Used to determine an interface's **MAC address** when its **IP address** is known.
    - Each host/router maintains an **ARP table** of IP/MAC address mappings with a TTL (Time To Live, typically 20 min).
    - **ARP Protocol Action:** Sender broadcasts an ARP query (destination MAC = `FF-FF-FF-FF-FF-FF`) containing the target IP address. The target node replies directly with an ARP response containing its MAC address.
- **Routing to Another Subnet:** A detailed walkthrough of frame and datagram address changes when moving a packet from host A to host B via router R. The IP addresses remain constant (A to B), but MAC addresses change hop-by-hop (A to R, then R to B).
- **Ethernet:**
    - The "dominant" wired LAN technology (10 Mbps – 400 Gbps).
    - **Topology:** Originally **bus** (single collision domain), now predominantly **switched** (active link-layer 2 switch with each spoke being its own collision domain).
    - **Connectionless, Unreliable:** No handshaking (connectionless) and no ACKs/NAKs; relies on higher layers (e.g., TCP) for recovery.
    - **MAC Protocol:** Unslotted CSMA/CD with binary backoff.
    - **Frame Structure:** Includes **Preamble** (synchronization), 6-byte **Source/Dest MAC addresses**, **Type** (for demultiplexing to higher layer protocol, e.g., IP), **Data (payload)**, and **CRC**.

### VI. Ethernet Switches

- **Switches:** Link-layer devices that store and forward Ethernet frames and act as both client and server (in the sense of link layer protocols).
- **Characteristics:** **Transparent** (hosts are unaware of them), **plug-and-play**, and **self-learning** (no configuration needed).
- **Operation:** Hosts have dedicated, full-duplex connections to the switch (no collisions on these links). The switch buffers packets.
- **Switch Table:** Contains entries: (MAC address, interface, time stamp).
- **Self-Learning Mechanism:** When a frame arrives, the switch records the sender's MAC address and incoming interface in its table.
- **Frame Filtering/Forwarding:**
    1. Record sender information.
    2. Index the table using the destination MAC address.
    3. If the destination is known: if it's on the arriving segment, the frame is **dropped** (filtered); otherwise, it is **forwarded** on the indicated interface.
    4. If the destination is unknown: the frame is **flooded** (forwarded on all interfaces except the arriving one).
- **Switches vs. Routers:** Both are store-and-forward devices. **Routers** are network-layer devices, use IP addresses, and compute tables via routing algorithms. **Switches** are link-layer devices, use MAC addresses, and learn tables via flooding/learning.

### VII. Virtual LANs (VLANs)

- **Motivation:** Standard LANs (single broadcast domain) face scaling, efficiency, security, and privacy issues, as all layer-2 broadcast traffic (ARP, DHCP, unknown MAC) must cross the entire LAN. Administrative issues arise when users physically move but need to remain logically connected to their original network.
- **Definition:** **VLANs** (Virtual Local Area Networks) are configured over a single physical LAN infrastructure, defining multiple virtual LANs.
- **Port-based VLANs:** Switch ports are logically grouped to make a single physical switch operate as multiple virtual switches (e.g., ports 1-8 for EE, 9-15 for CS).
    - **Traffic isolation:** Traffic to/from ports 1-8 can only reach ports 1-8.
    - **Forwarding between VLANs:** Requires **routing** (often combined switch-plus-router vendor solutions).
    - Membership can be based on MAC addresses, not just switch ports.
- **VLANs Spanning Multiple Switches:** A **trunk port** carries frames between VLANs defined over multiple switches.
- **802.1Q Protocol:** Used for frames forwarded over trunk ports; it adds extra header fields, including a **12-bit VLAN ID field** and a **3-bit priority field**.
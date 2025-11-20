- [ ] Chapter Goal: Get a "feel" and "big picture" introduction to terminology.

- [ ] Internet Views: "Nuts and Bolts" view (hardware components) and "Services" view (infrastructure providing services).

- [ ] Key Components: Hosts (end systems), Packet switches (routers, switches), Communication links (fiber, copper, radio, satellite), Transmission rate (bandwidth).

- [ ] Internet Structure: "Network of networks," interconnected ISPs.

- [ ] Network Edge: Hosts (clients and servers), Servers often residing in data centers.

- [ ] Access Networks: Residential, institutional, and mobile networks (WiFi, 4G/5G).

- [ ] Home Network Components: Modem (cable/DSL), Router/firewall/NAT, Wired Ethernet, WiFi wireless access point.

- [ ] Wireless Access: WLANs (802.11b/g/n), Wide-area cellular networks (4G/5G).

- [ ] Host Sending Function: Breaking application messages into packets (length L bits) and transmitting at rate R, calculating packet transmission delay ().

- [ ] Physical Media: Guided (copper, fiber, coax) and Unguided (radio), including twisted pair (Cat 5, 6), fiber optic cable (low error rate, high speed), and wireless radio (WLAN, cellular, satellite).

- [ ] Packet Switching: Breaking messages into packets, Routers forward packets.

  - [ ] Forwarding (Switching): Local action, moving packets from input to output link.

  - [ ] Routing: Global action, determining source-destination paths using routing algorithms.

  - [ ] Store-and-Forward: Entire packet must arrive before transmission starts on next link.

  - [ ] Queueing and Loss: Packets queue when arrival rate exceeds transmission rate; loss occurs if buffer/memory fills up.

- [ ] Circuit Switching: End-to-end resources reserved for a "call" (dedicated resources, guaranteed performance).

  - [ ] Multiplexing Types: Frequency Division Multiplexing (FDM), Time Division Multiplexing (TDM).

  - [ ] Comparison: Packet switching is better for "bursty" data due to resource sharing.

- [ ] Internet Structure: Involves Access ISPs, Local/Regional ISPs, Tier-1 Commercial ISPs, Internet Exchange Points (IXPs), and Content Provider Networks (e.g., Google, Akamai).

**IV. Performance**

- [ ] Packet Delay Sources (dnodal): Nodal processing delay (), Queueing delay (), Transmission delay (), and Propagation delay ().

- [ ] Traffic Intensity: Ratio . If this approaches 1, queueing delay becomes large; if , delay is infinite.

- [ ] Traceroute: Program used to measure delays along the path to a destination using probes.

- [ ] Packet Loss: Occurs when packets arrive at a full buffer.

- [ ] Throughput: Rate at which bits are transferred (instantaneous vs. average), constrained by the **bottleneck link**.

- [ ] Security Threats: Packet "sniffing" (reading all packets via promiscuous interface), IP spoofing (injection of packets with false source address), Denial of Service (DoS) attacks.

- [ ] Lines of Defense: Authentication, Confidentiality (encryption), Integrity checks (digital signatures), Access restrictions (VPNs), and Firewalls.

- [ ] Protocol Layers: Organization principle (layers implement services, relying on services below).

- [ ] Internet Protocol Stack (5 layers): Application, Transport, Network, Link, Physical.

- [ ] ISO/OSI Reference Model (7 layers): Adds Presentation and Session layers (often implemented in the application layer in the Internet stack).

- [ ] Internet History: Key periods from early packet-switching (ARPAnet, 1961-72), Internetworking (Cerf and Kahn principles), TCP/IP deployment (1980s), Commercialization/Web (1990s), to modern scale, SDN, mobility, and cloud computing (2005-present).

- [ ] Application Architectures: Client-Server (always-on server, often data centers) and Peer-to-Peer (P2P, arbitrary communication, self-scalable).

- [ ] Process Communication: Processes exchange messages via a **socket** (the interface between the application process and transport protocol).

- [ ] Addressing: Requires both Host IP address and Port Number (e.g., HTTP: 80, Mail: 25).

- [ ] Application Protocols: Define message types, syntax, semantics, and rules; classified as **open** (RFCs, HTTP) or **proprietary** (Skype, Zoom).

- [ ] Transport Service Requirements: Reliability (data integrity), Timing, Throughput (minimum/elastic), and Security.

- [ ] Internet Transport Services: TCP (Reliable, connection-oriented, flow control, congestion control) and UDP (Unreliable, connectionless, no control).

- [ ] Security: Transport Layer Security (TLS) provides encryption and authentication over TCP, often implemented in the application layer.

- [ ] Socket Programming: Uses UDP sockets (connectionless, datagrams) or TCP sockets (connection-oriented, reliable byte-stream).

- [ ] Timeouts: Used extensively in networking; setting timeouts using `settimeout()` and handling exceptions via `try-except` blocks.

- [ ] HTTP Overview: Client/server model, uses TCP (Port 80), is **stateless**.

- [ ] Connections: Non-persistent HTTP (1 object/connection, 2 RTTs + transmission time) vs. Persistent HTTP (multiple objects/connection, reduced delay).

- [ ] HTTP Messages: Request message (uses methods like GET, POST, HEAD, PUT) and Response message (includes status codes).

- [ ] Status Codes: E.g., 200 OK, 301 Moved Permanently, 404 Not Found.

- [ ] Web Caches (Proxy Servers): Reduce response time and traffic on access links.

- [ ] HTTP/2: Mitigates Head-of-Line (HOL) blocking using **frames** to interleave object delivery.

- [ ] HTTP/3: Uses the **QUIC** protocol over UDP to improve security and handling of packet loss.

- [ ] E-mail Components: User agents, Mail servers, and SMTP.

- [ ] SMTP: Uses TCP port 25, is a **client push** protocol (unlike HTTP pull), transfers mail directly between servers in 3 phases, messages must be 7-bit ASCII.

- [ ] Mail Access Protocols: Used to retrieve mail from the server (e.g., IMAP, HTTP for web-based mail).

- [ ] DNS (Domain Name System): Distributed, hierarchical application-layer protocol and database providing **hostname-to-IP address translation**, aliasing, and load distribution.

- [ ] DNS Hierarchy: Root DNS Servers, Top-Level Domain (TLD) Servers, and Authoritative DNS Servers. Local DNS servers cache mappings.

- [ ] Query Types: Iterated Query (contacted server gives name of next server) and Recursive Query (contacted server handles resolution burden).

- [ ] DNS Records (RR): Stored records include Type A (Hostname IP), NS (Authoritative server), CNAME (Alias Canonical name), and MX (Mail server).

- [ ] DNS Security: Threats include DDoS attacks and Spoofing/Cache Poisoning. DNSSEC provides authentication.

- [ ] P2P File Distribution (BitTorrent): Files divided into chunks, peers exchange chunks. Tracker coordinates peers. Uses a **tit-for-tat** strategy (uploading to peers who upload fastest) and "optimistically unchokes" peers periodically.

- [ ] Video Streaming: Encoding involves spatial and temporal coding; uses Constant Bit Rate (CBR) or Variable Bit Rate (VBR).

- [ ] Streaming Challenges: Client-side **buffering** is required to handle network delay variability (**jitter**) and maintain the continuous playout constraint.

- [ ] DASH (Dynamic, Adaptive Streaming over HTTP): Client requests chunks encoded at variable rates based on estimated bandwidth, informed by a manifest file.

- [ ] CDNs (Content Distribution Networks): Geographically distributed server sites to handle scale. Architectures include "enter deep" and "bring home".

- [ ] Transport Layer Function: Provides **logical communication** between application processes in different hosts.

- [ ] UDP Demultiplexing: Uses only the **destination port number**.

- [ ] TCP Demultiplexing: Uses the **4-tuple** (source IP, source port, destination IP, destination port).

- [ ] UDP Characteristics: Connectionless, unreliable, best-effort. No flow control or congestion control. Used for DNS, SNMP, and HTTP/3.

- [ ] UDP Checksum: Detects bit errors in the segment content using one's complement sum.

- [ ] Reliable Data Transfer (RDT): Implemented using Checksums, Sequence Numbers, ACKs/NAKs, and Timers.

  - [ ] Stop-and-Wait Protocols: rdt 2.0 (handles errors with ACKs/NAKs), rdt 2.1 (adds sequence numbers 0, 1), rdt 3.0 (adds timer to handle loss, performance is poor).

  - [ ] Pipelining: Allows multiple unACKed packets in flight to increase channel utilization.

  - [ ] Go-Back-N (GBN): Uses **cumulative ACKs**. On timeout, retransmits the lost packet and all subsequent packets in the window.

  - [ ] Selective Repeat (SR): Receiver individually ACKs correctly received packets; sender retransmits only unACKed packets.

- [ ] TCP Characteristics: Point-to-point, full duplex, reliable, in-order **byte stream**, connection-oriented, flow control, congestion control, uses cumulative ACKs.

- [ ] Segment Structure: Contains Sequence Number (first byte index), Acknowledgement Number (next expected byte), and **Receive Window (rwnd)**.

- [ ] RTT and Timeout: RTT is estimated using Exponential Weighted Moving Average (EWMA) of SampleRTT. Timeout is set based on .

- [ ] Fast Retransmit: Retransmitting the missing segment immediately upon receiving **three duplicate ACKs**.

- [ ] Flow Control: Receiver controls the sender by advertising the size of its free buffer space in the **rwnd** field, preventing buffer overflow.

- [ ] Connection Management: Established via a **3-Way Handshake** (SYN SYNACK ACK) to agree on initial sequence numbers. Closed via FIN bits.

- [ ] Congestion: Defined as too many sources sending too fast, leading to long delays and packet loss. Costs include wasted capacity due to unnecessary retransmissions.

- [ ] Approaches: End-end congestion control (inferred from loss/delay, used by TCP) vs. Network-assisted (explicit feedback from routers, e.g., ECN).

- [ ] TCP AIMD (Additive Increase, Multiplicative Decrease): Probes for bandwidth by linearly increasing the sending rate (Additive Increase) until loss, and cutting the rate sharply (Multiplicative Decrease) upon loss.

- [ ] Congestion Window (cwnd): Limits TCP sender transmission; TCP Rate cwnd/RTT.

- [ ] Phases: Slow Start (exponential increase until `ssthresh`), Congestion Avoidance (linear increase).

- [ ] TCP Variants: TCP CUBIC (default in Linux) modifies AIMD probing.

- [ ] Delay-based Control: Algorithms like BBR try to fill the pipe without causing high delays/loss, using as a baseline.

- [ ] Fairness: TCP aims for fair sharing (R/K), but is undermined by UDP flows (which lack congestion control) and parallel TCP connections.

- [ ] QUIC: Evolving transport-layer functionality running over UDP (used by HTTP/3). Provides 1-RTT connection setup and avoids HOL blocking across application streams.

- [ ] Key Functions: Forwarding (local, data plane) and Routing (global, control plane).

- [ ] Data Plane: Local function determining how datagrams move from input to output port.

- [ ] Control Plane: Network-wide logic determining the end-end path. Can be traditional (in routers) or Software-Defined Networking (SDN, implemented in remote controller).

- [ ] Internet Service Model: "Best effort," offering no guarantees on performance, loss, order, or bandwidth.

- [ ] Router Components: Input ports, Output ports, High-speed switching fabric, Routing processor.

- [ ] Input Ports: Perform physical/link layer processing, queueing, and **decentralized switching** (lookup/forwarding).

- [ ] Longest Prefix Matching: Rule for finding the correct entry in the forwarding table. Implemented efficiently using TCAMs.

- [ ] Switching Fabrics: Mechanisms to move packets internally (via memory, via bus, or via interconnection networks like Crossbar/Clos).

- [ ] Queueing Issues: Head-of-the-Line (HOL) blocking (in input port queueing), Output port queueing (leads to delay and loss if buffers overflow).

- [ ] Buffer Management: Drop policies (tail drop, priority drop) and Marking policies (ECN, RED).

- [ ] Packet Scheduling: Policies to decide next packet transmission: FCFS, Priority scheduling, Round Robin (RR), Weighted Fair Queueing (WFQ).

- [ ] Network Neutrality: Principles regarding resource allocation (no blocking, no throttling, no paid prioritization). Regulatory classification (Title I vs. Title II) matters greatly.

- [ ] IP Addressing: 32-bit identifier for a host/router interface.

- [ ] Subnets: Network partitions where devices can reach each other without an intervening router. Defined by a subnet part and a host part using a subnet mask.

- [ ] CIDR (Classless InterDomain Routing): Address format , allowing arbitrary length subnet portions.

- [ ] DHCP (Dynamic Host Configuration Protocol): Allows hosts to dynamically obtain an IP address, first-hop router address, DNS server address, and network mask upon joining a network.

- [ ] Hierarchical Addressing: Allows ISPs to allocate blocks and enables **route aggregation**. Allocation managed by **ICANN**.

- [ ] IPv6 Transition: Use of **tunneling**, where IPv6 datagrams are encapsulated as payloads inside IPv4 datagrams, due to mixed IPv4/IPv6 networks.

- [ ] OpenFlow: A protocol implementation of match+action.

- [ ] Middleboxes: Intermediary devices performing non-standard IP router functions (e.g., NAT, Firewalls, Load Balancers, Caches). Trend toward **Network Functions Virtualization (NFV)** and innovation in software.

- [ ] Architectural Principles: IP hourglass (IP as the narrow waist) and the **End-End Argument** (intelligence/complexity residing at the network edge).

- [ ] Terminology: Nodes (hosts/routers) connected by Links (wired/wireless). Layer-2 packets are called **frames**.

- [ ] Functions: Transfer datagrams between **physically adjacent nodes**. Services include framing, link access, reliable delivery (optional), flow control, error detection, and error correction.

- [ ] Implementation: Typically in the **Network Interface Card (NIC)**, implementing both link and physical layers.

- [ ] Error Detection: Use of EDC (Error Detection and Correction bits) to detect flipped bits caused by noise.

- [ ] Parity Checking: Single-bit parity (detects single error); Two-dimensional bit parity (detects and corrects single errors).

- [ ] CRC (Cyclic Redundancy Check): Powerful error detection technique widely used in Ethernet and WiFi.

- [ ] MAC Taxonomy: Channel partitioning, Random access, and "Taking turns" protocols.

- [ ] Channel Partitioning: TDMA (fixed time slots) and FDMA (fixed frequency bands).

- [ ] Random Access Protocols: Nodes transmit at full rate and recover from collisions. Examples include ALOHA, Slotted ALOHA (max efficiency 37%), Pure ALOHA (max efficiency 18%).

- [ ] CSMA (Carrier Sense Multiple Access): Listen before transmitting; defer if busy.

- [ ] CSMA/CD (Collision Detection): Aborts transmission upon detection of collision (used by Ethernet). Uses binary (exponential) backoff before retransmission.

- [ ] "Taking Turns": Polling (master invites slaves) and Token Passing (control token circulated).

- [ ] DOCSIS (Cable Access): Uses FDM downstream, and a mix of TDM and random access (contention slots) upstream.

- [ ] MAC Addresses (Physical/Ethernet Address): 48-bit addresses unique to each interface, used for Layer 2 forwarding within a subnet. Provide portability.

- [ ] ARP (Address Resolution Protocol): Used to dynamically discover a node's **MAC address** given its IP address on the same LAN. Information stored in an ARP table.

- [ ] Routing Walkthrough: Demonstration that **IP addresses remain constant** end-to-end, while **MAC addresses change** hop-by-hop between nodes and routers.

- [ ] Ethernet Frame: Unreliable and connectionless. Includes Preamble, MAC Addresses, Type (for demultiplexing), Data, and CRC.

- [ ] Ethernet Switch: Link-layer device (L2). Is **transparent** (hosts unaware) and **plug-and-play/self-learning**. Buffers frames and selectively forwards them, creating separate collision domains.

- [ ] Switch Table: Stores (MAC address, interface, time stamp) mappings.

- [ ] Forwarding Logic: Learns sender location; filters/forwards selectively; **floods** if destination address is unknown.

- [ ] VLANs (Virtual LANs): Define multiple virtual networks over a single physical infrastructure, primarily for network segmentation/traffic isolation in corporate environments.

- [ ] Port-based VLANs: Group switch ports to act as virtual switches.

- [ ] Network Modeling: Key steps include analyzing needs (including IP address availability), ensuring network/physical security ("security onion model"), planning for resiliency (recovery plans), and ensuring traceability (debugging).

- [ ] Network Segmentation: Primarily handled via **VLANs** in corporate campuses to avoid extensive physical cabling.

- [ ] Corporate Traffic Profile: Majority uses **IPv4 and IPv6** protocols over Ethernet and WiFi links, dominated by **TCP** and **HTTPS** traffic.

- [ ] Key Network Services: Address resolution (DNS, ARP), Network support (DHCP, NAT, Routing, VPN), Security (IDS, Firewalls), Authentication (MFA, Active Directory), Time (NTP, SNTP), HTTPS, and SMTP.

- [ ] Time Services: NTP/SNTP protocols are critical; need reputable/secure sources (GNSS, national sources) and backup services for robust certificate validation.

- [ ] Split DNS: Provides different DNS views for public/internal networks to hide internal networks and simplify internal domain names (requires corporate PKI for HTTPS/TLS).

- [ ] Virtualization: Common environments include Broadcom's vSphere, cloud virtual machines, and Kubernetes (often advised to use managed solutions). Enterprises often use a mixed cloud/on-prem strategy.

- [ ] Maintenance/Resiliency: Importance of recovery plans for critical systems (e.g., lessons from large security software failures).

- [ ] Global IPv6 Mandates: Various governments (China, India, USA) are setting firm deadlines for transition and IPv6-only operation (e.g., US Federal systems targeting 80% IPv6-only by FY 2025). Procurement processes increasingly require IPv6 competency.

- [ ] Transition Strategy: It is advised to start new initiatives with IPv6, utilize dual-stack cloud services and CDNs, and adopt **native IPv6 (dual-stack)** as the preferable solution.

- [ ] Transition Methods: **NAT64** (IPv6 IPv4 access) can be used, often paired with DNS64 for automated translation.

- [ ] Observed Benefit: Vaisala testing showed faster web browsing, attributed to **RFC 6555 (Happy Eyeballs)**.
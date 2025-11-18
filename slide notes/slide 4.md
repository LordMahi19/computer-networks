The sources derived from **"slide 4.pdf"** detail the **Network Layer's Data Plane**, covering the fundamental functions of forwarding, router architecture, the Internet Protocol (IP), addressing schemes, and the concepts of generalized forwarding and middleboxes.

Here are all the categories and topics mentioned in "slide 4.pdf":

### I. Network Layer: Goals and Overview

- **Goal:** Understand principles behind network layer services, focusing on the **data plane**.
- **Key Concepts:** Network layer service models, forwarding versus routing, how a router works, addressing, generalized forwarding, and Internet architecture.
- **Instantiation/Implementation:** IP protocol, NAT, and middleboxes.
- **Two Key Functions:**
    - **Forwarding** (Data Plane): Local, per-router function moving packets from an input link to the appropriate output link. Analogy: getting through a single interchange.
    - **Routing** (Control Plane): Network-wide logic determining the route taken by packets from source to destination. Analogy: planning a trip.

### II. Network-Layer Service Model

- **Service Examples for Individual Datagrams:** Guaranteed delivery; guaranteed delivery with less than 40 msec delay.
- **Service Examples for a Flow of Datagrams:** In-order datagram delivery; guaranteed minimum bandwidth to flow; restrictions on changes in inter-packet spacing.
- **Internet's Service Model:** **"Best effort"** service model.
    - No guarantees on: successful datagram delivery, timing/order of delivery, or bandwidth available.
- **Service Model Comparison Table:** Compares Network Architecture (Internet, ATM, Intserv, Diffserv) against Service Model, Bandwidth, Loss, Order, and Timing guarantees.
- **Reflections on Best-Effort Service:** Simplicity allowed wide deployment, sufficient bandwidth provisioning helps real-time apps, and congestion control of "elastic" services helps.

### III. Router Architecture and Internals

- **Router Architecture Overview:** High-level view includes **router input ports**, **switching fabric**, **router output ports**, and a **routing processor**.
    - Data plane (forwarding) operates in hardware in the nanosecond timeframe.
    - Control plane (routing, management) operates in software in the millisecond timeframe.
- **Input Port Functions:** Line termination (physical layer), link layer protocol (receive, e.g., Ethernet), lookup/forwarding, and queuing.
    - **Decentralized Switching:** Use header fields and the forwarding table for lookup ("match plus action") to complete processing at 'line speed'.
    - **Input Port Queuing:** Occurs if datagrams arrive faster than the rate into the switch fabric.
- **Forwarding Mechanisms:**
    - **Destination-based forwarding:** Traditional forwarding based only on the destination IP address.
    - **Generalized forwarding:** Forwarding based on any set of header fields.
- **Longest Prefix Matching:** When looking up a forwarding table entry, the longest address prefix that matches the destination address is used. Often performed using **Ternary Content Addressable Memories (TCAMs)**.
- **Switching Fabrics (Transfer Packet from Input to Output):**
    - **Switching via Memory (First Generation):** Packet copied to system memory; speed limited by memory bandwidth (2 bus crossings per datagram).
    - **Switching via a Bus:** Datagram copied from input port memory to output port memory via a shared bus; speed limited by bus bandwidth (e.g., 32 Gbps).
    - **Switching via Interconnection Network (Crossbar, Clos):** Exploits parallelism, often by fragmenting datagrams into fixed-length cells, switching cells through the fabric, and reassembling at the exit. Example: Cisco CRS router uses multiple switching planes and 3-stage interconnection networks.
- **Output Port Queuing:**
    - **Buffering:** Required when datagrams arrive from the fabric faster than the link transmission rate. Datagrams can be lost if buffers overflow.
    - **Head-of-the-Line (HOL) blocking:** Can occur at the input port if a queued datagram prevents others from moving forward.
    - **Buffering Size:** RFC 3439 suggests buffering equal to "typical" RTT (e.g., 250 msec) times link capacity $C$. Too much buffering increases delays.
- **Buffer Management:**
    - **Drop Policies:** Determines which packet to add/drop when buffers are full (e.g., **tail drop**, priority).
    - **Marking:** Which packets to mark to signal congestion (e.g., ECN, RED).
- **Packet Scheduling:** Deciding which packet to send next.
    - **FCFS (First Come, First Served) / FIFO**.
    - **Priority scheduling:** Traffic queued by class; highest priority packet is sent next.
    - **Round Robin (RR) scheduling:** Server cyclically scans class queues, sending one packet from each class.
    - **Weighted Fair Queuing (WFQ):** Generalized Round Robin providing minimum bandwidth guarantees.
- **Sidebar: Network Neutrality:** Discusses the technical (scheduling, buffer management) and socio-economic principles (free speech, innovation). Mentions US FCC "clear, bright line" rules (no blocking, no throttling, no paid prioritization) and the regulatory distinction between ISP as "telecommunications service" vs. "information service".

### IV. IP: The Internet Protocol

- **Host/Router Functions:** IP protocol defines datagram format and addressing; ICMP handles error reporting/router signaling; forwarding tables store path-selection algorithms (from routing protocols or SDN controller).
- **IP Datagram Format (IPv4):** Includes fields for **version** (4 bits), **header length**, **type of service** (Diffserv/ECN), **total length**, **16-bit identifier**, **flags**, **fragment offset**, **time to live (TTL)**, **upper layer protocol** (e.g., TCP/UDP), **header checksum**, **source IP address**, **destination IP address**, and **options**.
    - Header overhead (TCP + IP) is typically 40 bytes plus application layer overhead.
- **IP Addressing: IPv4**
    - **IP Address:** 32-bit identifier associated with each host or router interface.
    - **Interface:** Connection between a host/router and a physical link.
    - **Dotted-decimal notation** is used (e.g., 223.1.1.1).
    - **Subnets:** Device interfaces that can physically reach each other without passing through an intervening router.
    - **Subnet Mask:** Notation like `/24` specifies the high-order bits that define the subnet part of the IP address.
    - **Forwarding (Routing) Table:** Uses the destination network (including mask), next router, and outgoing interface for forwarding, emphasizing **Longest Prefix Match**.
    - **CIDR (Classless InterDomain Routing):** Subnet portion of address can be of arbitrary length (e.g., a.b.c.d/x).
    - **Address Acquisition (Host):** Hard-coded or dynamically obtained using **DHCP**.
    - **DHCP (Dynamic Host Configuration Protocol):** Allows hosts to dynamically obtain an IP address, supporting "plug-and-play" and address reuse. Involves a four-step process: Discover (optional), Offer (optional), Request, and ACK. DHCP can also return the address of the first-hop router, DNS server, and network mask.
    - **Address Acquisition (Network/ISP):** Networks are allocated a portion of their provider ISP's address space. ICANN manages IP address allocation through regional registries.
    - **Hierarchical Addressing (Route Aggregation):** Allows ISPs to advertise large blocks of addresses efficiently.
- **NAT (Network Address Translation):**
    - Allows all devices in a local network to share a single IPv4 address (e.g., 138.76.29.7).
    - Devices inside the local net use **"private" IP address space** (e.g., 10/8).
    - **Advantages:** Only one public IP address needed, local address changes don't affect the outside world, ISP changes don't affect internal addresses, and security (internal devices are not directly visible).
    - **Implementation:** The NAT router replaces the source (IP, port #) of outgoing datagrams with the (NAT IP address, new port #), and remembers the translation pair in the **NAT translation table**.
    - **Controversy:** NAT is controversial because it violates the end-to-end argument and requires routers to process up to layer 4 (port numbers). However, it is extensively used in home, institutional, and cellular nets.
- **IPv6:**
    - **Motivation:** IPv4 address space exhaustion and need for speed/fixed-length header.
    - **Address Space:** 128-bit address space.
    - **Datagram Format:** Fixed-length 40-byte header. Missing features compared to IPv4: no checksum, no fragmentation/reassembly, and options are moved to upper-layer protocols. Includes fields for **flow label** and **payload length**.
    - **Transition (IPv4 to IPv6):** Must operate with mixed IPv4 and IPv6 routers using **tunneling**.
    - **Tunneling:** IPv6 datagram is carried as a payload inside an IPv4 datagram among IPv4 routers ("packet within a packet"). Used extensively in other contexts (e.g., 4G/5G).
    - **Adoption:** Long deployment time (25 years and counting).

### V. Generalized Forwarding and SDN

- **Generalized Forwarding ("Match Plus Action"):** An abstraction where routers match bits in an arriving packet and take a defined action.
    - **Flow:** Defined by header field values (link-, network-, transport-layer fields).
    - **Rules (Flow Table Abstraction):** Consist of a **match** (pattern values in header fields), **actions** (drop, forward, modify, send to controller), **priority**, and **counters**.
- **OpenFlow:** Implementation of the match+action principle.
    - **Match Fields:** Includes ingress port, MAC source/dest, IP source/dest, TCP/UDP ports, etc..
    - **Actions:** Forward to port(s), drop, modify fields, or encapsulate and forward to the controller.
    - **Unification:** The match+action abstraction unifies different device types (Router, Switch, Firewall, NAT).
    - **SDN Context:** The remote controller computes and installs forwarding tables (flow tables) in routers.

### VI. Middleboxes

- **Definition:** "Any intermediary box performing functions apart from normal, standard functions of an IP router" on the data path.
- **Examples:** NAT (home, cellular, institutional), Firewalls, IDS, Load balancers, Caches, and Application-specific devices.
- **Architectural Shifts:** Moving away from proprietary hardware toward **"whitebox" hardware** and software-defined networking (**SDN**).
- **Network Functions Virtualization (NFV):** Running programmable services over white box networking, computation, and storage.
- **The IP Hourglass:** Highlights IP as the single network layer protocol (the "thin waist") that must be implemented by every device, allowing many protocols above and below it.
- **The End-End Argument:** Some network functions (like reliability or congestion control) can be implemented in the network or at the edge. The argument states that the function can only be completely and correctly implemented with the help of the application at the endpoints.
- **Where is the Intelligence?** Modern networks post-2005 are characterized by **programmable network devices** and intelligence/computing/massive application-level infrastructure at the edge.
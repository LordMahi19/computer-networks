# Topics
- What is the Internet? What is a protocol? 
- Network edge: hosts, access network, physical media 
- Network core: packet/circuit switching, internet structure 
- Performance: loss, delay, throughput Protocol layers, service models Security History

# Explanation
The sources, The sources, which consist of excerpts from "slide 1.pdf," cover a wide range of topics related to computer networks and the Internet, primarily aligned with the introduction chapter of a textbook.

Here are all the topics and subtopics mentioned in "slide 1.pdf":

### I. Introduction and Chapter Goal

- **Chapter 1 Introduction**.
- Course: **CT60A5540 Computer networks and Internet**.
- Textbook: _Computer Networking: A Top-Down Approach_ 8th edition by Jim Kurose and Keith Ross.
- Chapter goal: to get a "feel" and "big picture" introduction to terminology.
- Course structure: more depth and detail will follow later in the course.
- **Overview/roadmap** for the chapter, which includes many of the topics listed below.

### II. The Internet and Protocols

- **What is the Internet?**.
- **The Internet: a “nuts and bolts” view**.
    - **Internet components:**
        - Billions of connected computing devices, referred to as **hosts** or **end systems**, running network apps at the Internet's "edge".
        - **Packet switches** (routers, switches) that forward packets (chunks of data).
        - **Communication links** (fiber, copper, radio, satellite).
        - Transmission rate, also known as **bandwidth**.
        - **Networks**, which are collections of devices, routers, and links managed by an organization.
    - **Internet structure visualization:** mobile network, home network, enterprise network, national or global ISP, local or regional ISP, datacenter network, and content provider network.
    - The Internet is a **“network of networks”** consisting of interconnected ISPs.
- **“Fun” Internet-connected devices** (e.g., Web-enabled toaster, Internet phones, Slingbox, Security Camera, IP picture frame, Internet refrigerator, Tweet-a-watt, sensorized bed mattress, Amazon Echo, Gaming devices, cars, scooters, bikes, Pacemaker & Monitor, AR devices, Fitbit, diapers).
- **The Internet: a “services” view**.
    - Infrastructure that provides services to applications: Web, streaming video, multimedia teleconferencing, email, games, e-commerce, social media, inter-connected appliances.
    - Provides a **programming interface** ("hooks") to distributed applications, analogous to postal service.
- **What is a protocol?**.
    - **Human protocols** (e.g., "what’s the time?", introductions).
    - **Network protocols** govern all communication activity in the Internet.
    - Protocols define the **format, order of messages** sent and received, and **actions taken** on transmission/receipt.
    - Examples of protocols: **HTTP** (Web), **streaming video**, **Skype**, **TCP**, **IP**, **WiFi**, **4/5G**, **Ethernet**.
- **Internet standards**:
    - **RFC: Request for Comments**.
    - **IETF: Internet Engineering Task Force**.

### III. Network Edge

- **Network edge: hosts, access network, physical media**.
- **Hosts**: clients and servers, with servers often residing in data centers.
- **Host sending function**: taking an application message, breaking it into **packets** (length $L$ bits), and transmitting the packet into the access network at a transmission rate $R$.
    - **Transmission delay**: $L \text{ (bits)} / R \text{ (bits/sec)}$.
- **Access networks and physical media**.
    - Connecting end systems to the edge router (Q: How to connect?).
    - Types of access networks: residential, institutional (school, company), and mobile (WiFi, 4G/5G).
    - **Cable-based access**.
        - Uses **frequency division multiplexing (FDM)**.
        - Data and TV transmitted at different frequencies over shared cable distribution network.
        - **HFC: hybrid fiber coax**.
        - Asymmetric transmission rates (e.g., up to 40 Mbps – 1.2 Gbps downstream).
    - **Digital Subscriber Line (DSL)**.
        - Uses existing telephone lines to a central office DSLAM.
        - Voice and data transmitted at different frequencies over a dedicated line.
        - Dedicated downstream (24-52 Mbps) and upstream (3.5-16 Mbps) transmission rates.
    - **Home networks**: typically include a cable or DSL modem, a router/firewall/NAT, wired Ethernet (1 Gbps), and WiFi wireless access point (54, 450 Mbps).
    - **Wireless access networks**.
        - Shared wireless access network connects end systems to a router via a base station/access point.
        - **Wireless local area networks (WLANs)** (e.g., 802.11b/g/n WiFi).
        - **Wide-area cellular access networks** (4G/5G cellular networks).
    - **Enterprise networks** (companies, universities).
        - Mix of wired/wireless link technologies, switches, and routers.
        - Ethernet (100Mbps, 1Gbps, 10Gbps wired access) and WiFi (11, 54, 450 Mbps wireless access).
    - **Data center networks**.
        - High-bandwidth links (10s to 100s Gbps) connecting hundreds to thousands of servers.
- **Links: physical media**.
    - **Bit**: propagates between transmitter/receiver pairs.
    - **Physical link**: what lies between transmitter and receiver.
    - **Guided media** (signals propagate in solid media: copper, fiber, coax).
        - **Twisted pair (TP)** (Category 5, Category 6 Ethernet).
        - **Coaxial cable** (two concentric copper conductors, bidirectional, broadband).
        - **Fiber optic cable** (glass fiber, high-speed point-to-point transmission (10s–100s Gbps), low error rate, immune to electromagnetic noise).
    - **Unguided media** (signals propagate freely, e.g., radio).
        - **Wireless radio** (signal carried in electromagnetic spectrum bands, broadcast, half-duplex).
        - Propagation environment effects: reflection, obstruction, interference/noise.
        - Radio link types: Wireless LAN (WiFi), wide-area (4G/5G cellular), Bluetooth, terrestrial microwave, satellite (e.g., Starlink).

### IV. Network Core

- **Network core: packet/circuit switching, internet structure**.
- **The network core**: mesh of interconnected routers.
- **Packet-switching**.
    - Hosts break application messages into packets, and the network forwards packets from router to router.
    - Two key network-core functions: **Routing** (global action: determining source-destination paths) and **Forwarding** (local action: moving arriving packets to the appropriate output link, aka "switching").
    - **Packet-switching: store-and-forward**: the entire packet must arrive at a router before transmission on the next link.
    - **Packet-switching: queueing**.
        - Queueing occurs when work arrives faster than it can be serviced.
        - **Packet queuing and loss**: packets queue if the arrival rate exceeds the transmission rate, and packets can be dropped (lost) if the router memory (buffer) fills up.
- **Alternative to packet switching: circuit switching**.
    - End-to-end resources are allocated and reserved for a call.
    - Dedicated resources mean no sharing, providing circuit-like (guaranteed) performance.
    - Circuit segment is idle if not used by the call.
    - Commonly used in traditional telephone networks.
    - **Circuit switching: FDM and TDM** (Frequency Division Multiplexing and Time Division Multiplexing).
- **Packet switching versus circuit switching**.
    - Packet switching is great for **“bursty” data**, allows **resource sharing**, and is simpler (no call setup).
    - Packet switching risks **excessive congestion** leading to delay and loss, requiring protocols for reliable data transfer and congestion control.
- **Internet structure: a “network of networks”**.
    - Hosts connect via access ISPs, which must be interconnected so any two hosts can exchange packets.
    - Evolution driven by economics and national policies.
    - Methods of interconnection: connecting each access ISP to one global transit ISP, using **Internet Exchange Points (IXPs)** and peering links between competitor ISPs, and the rise of **regional ISPs**.
    - **Content provider networks** (e.g., Google, Microsoft, Akamai) run their own networks to bring services closer to end users.
    - The "center" consists of a small number of well-connected large networks, including **“tier-1” commercial ISPs** and content provider networks.

### V. Performance

- **Performance: loss, delay, throughput**.
- **Packet delay and loss**: packets queue in router buffers, and loss occurs when memory fills up.
    - Lost packets may be retransmitted by the previous node, source end system, or not at all.
- **Packet delay: four sources**.
    - **$d_{\text{proc}}$ (nodal processing)**: checking bit errors, determining output link (typically < microsecs).
    - **$d_{\text{queue}}$ (queueing delay)**: time waiting at the output link for transmission, dependent on router congestion.
    - **$d_{\text{trans}}$ (transmission delay)**: $L/R$ (packet length / link transmission rate).
    - **$d_{\text{prop}}$ (propagation delay)**: $d/s$ (physical link length / propagation speed).
    - Total nodal delay: $d_{\text{nodal}} = d_{\text{proc}} + d_{\text{queue}} + d_{\text{trans}} + d_{\text{prop}}$.
    - **Caravan analogy** used to compare transmission delay and propagation delay.
- **Packet queueing delay (revisited)**.
    - Dependent on **traffic intensity** ($L a / R$, where $a$ is average arrival rate).
    - When traffic intensity is near 0, delay is small; when it approaches 1, delay is large; when it exceeds 1, average delay is infinite.
- **“Real” Internet delays and routes**: discussed using the **traceroute program** to measure delay from source to router.
- **Throughput**.
    - Rate (bits/time unit) at which bits are sent from sender to receiver.
    - Can be instantaneous or average.
    - The **bottleneck link** is the link on the end-to-end path that constrains the throughput.

### VI. Security

- **Security**.
- The Internet was not originally designed with much security in mind, relying on a vision of "mutually trusting users".
- Security considerations are needed in all layers.
- Topics include how bad guys attack networks, how to defend, and how to design immune architectures.
- **Bad guys actions/attacks**:
    - **Packet interception** via packet **“sniffing”** (e.g., using Wireshark software) on broadcast media.
    - **Fake identity** via **IP spoofing** (injection of a packet with a false source address).
    - **Denial of Service (DoS)**: overwhelming resources (server, bandwidth) with bogus traffic.
- **Lines of defense**:
    - **Authentication** (proving identity, noting lack of hardware assist in traditional Internet).
    - **Confidentiality** via encryption.
    - **Integrity checks** (digital signatures).
    - **Access restrictions** (password-protected VPNs).
    - **Firewalls** (specialized “middleboxes” that filter incoming packets and detect/react to DoS attacks).

### VII. Protocol Layers and Service Models

- **Protocol layers, service models**.
- Networks are complex (hosts, routers, links, applications, protocols, hardware, software).
- **Why layering?** Explicit structure allows identification and relationships of system pieces. Modularization eases maintenance and updating.
- **Example: organization of air travel** used to illustrate the concept of layers, where each layer implements a service relying on the layer below.
- **Layered Internet protocol stack**.
    - **Application layer**: supports network applications (HTTP, IMAP, SMTP, DNS).
    - **Transport layer**: process-process data transfer (TCP, UDP).
    - **Network layer**: routing of datagrams from source to destination (IP, routing protocols).
    - **Link layer**: data transfer between neighboring network elements (Ethernet, 802.11 (WiFi), PPP).
    - **Physical layer**: bits “on the wire”.
- **Services, Layering and Encapsulation**.
    - **Encapsulation**: a higher layer message is enclosed within the header of the next lower layer, analogous to Matryoshka dolls.
        - Application layer message $M$.
        - Transport layer segment: $H_t | M$.
        - Network layer datagram: $H_n | [H_t | M]$.
        - Link layer frame: $H_l | [H_n | [H_t | M]]$.
- **ISO/OSI reference model**.
    - The seven-layer model is discussed, noting that the **Presentation** and **Session** layers are "missing" from the Internet protocol stack, and their services must be implemented in the application layer if needed.

### VIII. History

- **History**.
- **1961–1972: Early packet-switching principles**.
    - 1961: Kleinrock shows effectiveness of packet-switching using queueing theory.
    - 1967: ARPAnet conceived.
    - 1969: First ARPAnet node operational.
    - 1972: ARPAnet public demo; **NCP** (Network Control Protocol) is the first host-host protocol; first e-mail program.
- **1972–1980: Internetworking, new and proprietary networks**.
    - 1974: Cerf and Kahn develop architecture for interconnecting networks.
    - 1976: **Ethernet** at Xerox PARC.
    - Cerf and Kahn’s internetworking principles: minimalism, autonomy, best-effort service model, stateless routing, decentralized control.
- **1980–1990: New protocols, a proliferation of networks**.
    - 1983: deployment of **TCP/IP**.
    - 1982: **smtp** e-mail protocol defined.
    - 1983: **DNS** defined.
    - 1988: TCP congestion control.
- **1990, 2000s: Commercialization, the Web, new applications**.
    - Early 1990s: ARPAnet decommissioned; NSF lifts restrictions on commercial use of NSFnet.
    - Early 1990s: **The Web** (HTML, HTTP by Berners-Lee; Mosaic, Netscape).
    - Late 1990s – 2000s: more killer apps (instant messaging, P2P file sharing); network security moves to the forefront.
- **2005–present: Scale, SDN, mobility, cloud**.
    - Aggressive deployment of **broadband home access**.
    - 2008: **software-defined networking (SDN)**.
    - Increasing ubiquity of high-speed wireless access: 4G/5G, WiFi.
    - Service providers (Google, FB, Microsoft) create their own networks to bypass the commercial Internet.
    - Enterprises running services in the **“cloud”** (e.g., Amazon Web Services, Microsoft Azure).
    - Rise of **smartphones**; more mobile than fixed devices on the Internet.

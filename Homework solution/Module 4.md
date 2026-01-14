# What is the role of the forwarding table within a router?
Within a router's **data plane**, the forwarding table (also called a flow table) performs the **local action** of transferring arriving packets from an input link to the appropriate output link. It operates on a **"match plus action"** principle, where header bits are compared against entries to determine if a packet should be forwarded, dropped, or modified. For traditional destination-based forwarding, routers utilize **longest prefix matching** to select the most specific entry for a packet's destination IP address. In **Software-Defined Networking (SDN)**, these tables are more flexible, matching against multiple header fields across different layers to enable complex network-wide behaviors. To ensure efficiency at "line speed," these tables are typically stored in **decentralized input port memory** and perform lookups in a **nanosecond timeframe**.
# question 1C
What are the smallest network masks (hold smallest possible number of hosts) for networks, where are only hosts 192.168.1.1, 192.168.1.3 and 192.168.1.4? What is smallest network mask for network with hosts 192.168.1.1-192.168.1.130? How about if we need to have at least 560 hosts in our subnetwork?

Based on the sources and standard networking principles, here are the smallest network masks (CIDR notation) for your scenarios. **CIDR** uses the format `a.b.c.d/x`, where `x` is the number of bits in the subnet portion.

- **For hosts 192.168.1.1, 192.168.1.3, and 192.168.1.4:** The smallest mask is **`/29`**. This provides 8 total addresses (3 host bits), which is the smallest block that can contain all three specific IPs. A `/30` only provides 4 total addresses (2 usable), which is insufficient.
- **For hosts 192.168.1.1 to 192.168.1.130:** The smallest mask is **`/24`**. You need to accommodate 130 hosts; a `/25` only provides 128 total addresses (typically 126 usable), while a `/24` provides 256.
- **For at least 560 hosts:** The smallest mask is **`/22`**. This provides 1,024 total addresses (10 host bits). The next size down, `/23`, only provides 512 addresses, which is not enough for 560 hosts.

---

**Note:** The sources define the `/x` notation and provide examples like `/24`, but the specific binary calculations for address capacity (e.g., $2^{10} = 1024$) are based on standard mathematical logic not explicitly detailed in the provided text. You may wish to independently verify these calculations.
# question 2
Switching in a router forwards data from an input port an output port. What is the advantage of switching via an interconnection network over switching via memory and switching via bus?

An **interconnection network** (such as a crossbar or multistage switch) provides a significant advantage over memory and bus-based switching by enabling **parallelism**.

- **Overcoming Bottlenecks:** Unlike switching via **memory** (limited by memory bandwidth and CPU control) or a **shared bus** (limited by bus contention where only one packet can transfer at a time), an interconnection network can forward **multiple packets simultaneously**.
- **Parallel Processing:** It often fragments datagrams into **fixed-length cells** to exploit this parallelism efficiently.
- **High Scalability:** This architecture allows for massive capacity; for example, the Cisco CRS router uses eight switching planes in parallel to achieve total capacities reaching **hundreds of Tbps**.

While highly efficient, if the fabric is slower than the combined input port speed, **Head-of-the-Line (HOL) blocking** can still occur, where a packet at the front of a queue prevents others behind it from being forwarded.
# question 3
Suppose an application generates chunks of 40 bytes of data every 20 msec, and each chunk gets encapsulated in a TCP segment and then an IP datagram. What percentage of each datagram will be overhead, and what percentage will be application data?

To determine the percentage of overhead and application data in the IP datagram, we must first calculate the total size of the datagram by accounting for the encapsulation process.

Based on the sources, the breakdown is as follows:

- **Application Data:** The application generates chunks of **40 bytes** of data.
- **TCP Overhead:** When the data is encapsulated into a TCP segment, a transport-layer header ($H_t$) is added. The standard TCP header is **20 bytes**.
- **IP Overhead:** When the TCP segment is further encapsulated into an IP datagram, a network-layer header ($H_n$) is added. The standard IP header is also **20 bytes**.

Total Datagram Calculation**

The total size of each IP datagram is the sum of the application data and the protocol overhead:

- Total size = 40 bytes (Data) + 20 bytes (TCP) + 20 bytes (IP) = **80 bytes**.

Percentage Breakdown**

- **Overhead Percentage:** The total protocol overhead is 40 bytes (20 bytes TCP + 20 bytes IP).
    - (40 bytes overhead / 80 bytes total) × 100 = **50% overhead**.
- **Application Data Percentage:** The application data is 40 bytes.
    - (40 bytes data / 80 bytes total) × 100 = **50% application data**.

In this scenario, exactly **50% of each datagram consists of protocol overhead**, while the remaining **50% is actual application data**.
# question 4
Suppose you purchase a wireless router and connect it to your cable modem. Also suppose that your ISP dynamically assigns your connected device (that is, your wireless router) one IP address. Also suppose that you have five PCs at home that use 802.11 to wirelessly connect to your wireless router. How are IP addresses assigned to the five PCs? Does the wireless router use NAT? Why or why not?

In the scenario described, IP addresses are assigned to the five PCs through a protocol called **Dynamic Host Configuration Protocol (DHCP)**, and the wireless router **does use Network Address Translation (NAT)** to manage their connection to the Internet,,.

IP Address Assignment via DHCP**

The five PCs obtain their IP addresses dynamically when they connect to the wireless network.

- **Built-in Server:** A home wireless router typically includes a **built-in DHCP server**,.
- **The Process:** When a PC joins the network, it broadcasts a "DHCP discover" message to find the server,. The router responds with a "DHCP offer" that includes an available IP address, a network mask, the address of the first-hop router (the default gateway), and the address of a DNS server,,.
- **Private Addresses:** These PCs are assigned **private IP addresses** (typically with prefixes like 10.0.0/8, 172.16/12, or 192.168/16) that are unique within your home network but are not visible or addressable by the outside world,.

The Role of NAT**

The wireless router uses NAT because your ISP has only assigned a **single public IP address** to your router, but you have multiple devices (the five PCs) that need to communicate with the Internet simultaneously,.

**How it works:**

- **Address Sharing:** NAT allows all devices on your local network to **share that one public IPv4 address**.
- **Translation Table:** The router maintains a **NAT translation table** to keep track of these connections,.
- **Outgoing Traffic:** When a PC sends a datagram to the Internet, the NAT router replaces the PC's private IP address and source port number with its own public IP address and a new, unique port number,.
- **Incoming Traffic:** When a response arrives from the Internet, the router uses the destination port number to look up the corresponding private IP address in its translation table and forwards the datagram to the correct PC,.

**Why NAT is used:** Beyond allowing multiple devices to share a single IP address, NAT provides **security** because devices inside the local network are not directly visible to the outside world. It also allows you to change the internal addresses of your devices or switch ISPs without needing to reconfigure the entire local network.
# What is meant by the term “route aggregation”? Why is it useful for a router to perform route aggregation?
**Route aggregation**, also known as route summarization, is a technique made possible by **hierarchical addressing** that allows a router to represent a large block of IP addresses as a single, summarized prefix.

What is Route Aggregation?

In a hierarchical addressing scheme, a provider (such as an ISP) is allocated a large block of contiguous addresses. The provider then allocates smaller sub-blocks of this address space to individual organizations. For example, a "Fly-By-Night-ISP" might own the large block `200.23.16.0/20` and assign smaller `/23` blocks within that range to various organizations (Organization 0, Organization 1, etc.).

Instead of advertising every individual organization's specific address to the rest of the Internet, the ISP’s router performs **route aggregation** by advertising only the single, shorter prefix (`200.23.16.0/20`). This single advertisement tells the rest of the world: "Send me anything with addresses beginning with 200.23.16.0/20".

Why is it Useful?

It is useful for a router to perform route aggregation for the following reasons:

- **Efficiency of Routing Advertisements:** It allows for the **efficient advertisement of routing information**. Without aggregation, routers across the global Internet would need to maintain an entry for every individual organization's network, which would be unmanageable.
- **Scalability of Forwarding Tables:** By summarizing many specific routes into one entry, it significantly reduces the size and complexity of forwarding tables in routers throughout the network core.
- **Hierarchical Organization:** It reflects the structure of the Internet where address space is distributed from providers to customers, ensuring that the network can continue to scale as more organizations join.
- **Support for Specific Routes:** Even with aggregation, the system is flexible. If an organization moves to a different ISP, the new ISP can advertise a **more specific route** (the longest prefix) for that specific organization. Routers will then use **longest prefix matching** to ensure traffic is sent to the correct location despite the existence of a broader aggregated route elsewhere.

# What is meant by a “plug-and-play” or “zeroconf” protocol?
A **“plug-and-play”** or **“zeroconf”** (zero-configuration) protocol allows a device to connect to a network and function automatically without manual intervention or configuration.

Key examples from the sources include:

- **DHCP (Dynamic Host Configuration Protocol):** This is a primary plug-and-play protocol that allows a host to dynamically obtain an **IP address**, **network mask**, **DNS server** details, and the address of its **first-hop router** when it joins a network. This automation is essential for supporting mobile users and allowing the efficient reuse of IP addresses.
- **Ethernet Switches:** These are described as plug-and-play, **self-learning** devices. They do not need to be configured because they automatically build their forwarding tables by observing the MAC addresses of incoming frames to determine which hosts are reachable via which interfaces.

In short, these protocols simplify networking by moving the "intelligence" of configuration to the background, allowing systems to be "transparent" to the user.
# What is a private network address? Should a datagram with a private network address ever be present in the larger public Internet? Explain.
A **private network address** is a 32-bit IP address within specific prefixes—**10/8, 172.16/12, and 192.168/16**—that is intended for use only within a local network. These addresses allow all devices in a local network, such as a home or office, to have their own unique identifiers while sharing a single public IPv4 address as seen by the outside world.

A datagram with a private network address **should never be present** in the larger public Internet. The following points from the sources explain why:

- **NAT Translation Requirement:** When a datagram leaves a local network, a **Network Address Translation (NAT) router** must replace the private source IP address and port number with the router's public NAT IP address and a new port number. For example, in a provided scenario, a host with the private address 10.0.0.1 has its source address changed to a public address like 138.76.29.7 before the datagram is sent into the rest of the Internet.
- **Restricted Usage:** The sources explicitly state that these private address spaces **"can only be used in local network"**. They are not globally unique, meaning many different local networks may use the same private addresses (e.g., 192.168.1.1) internally.
- **Security and Stealth:** One of the advantages of using private addresses is that devices inside a local network are **not directly addressable or visible** to the outside world. If private addresses were routable on the public Internet, this security benefit would be lost.
- **Address Exhaustion:** NAT was initially motivated by the exhaustion of the 32-bit IPv4 address space; allowing many devices to share one public IP through private addressing helps mitigate this shortage.

According to the sources, the NAT router maintains a **translation table** to ensure that incoming responses, which are sent to the public NAT address, are correctly mapped back to the appropriate internal private address.
# Compare and contrast the IPv4 and the IPv6 header fields. Do they have any fields in common?
While IPv6 was designed to streamline processing, it retains several functional similarities to IPv4.

Common Fields**

- **Version:** Both begin with a 4-bit version field to identify the protocol.
- **Addresses:** Both contain **Source and Destination Addresses**, though IPv4 uses 32 bits and IPv6 uses 128 bits.
- **Lifetime:** IPv4's **TTL (Time to Live)** is functionally equivalent to the IPv6 **Hop Limit**; both are decremented at each router to prevent infinite loops.
- **Length:** IPv4 has a **Total Length** field, while IPv6 has a **Payload Length** field.
- **Upper-Layer Protocol:** IPv4’s **Upper Layer** field serves the same purpose as the IPv6 **Next Header** field, identifying the encapsulated protocol (e.g., TCP or UDP).
- **Traffic Class:** IPv4's **Type of Service (ToS)** matches the functional intent of the IPv6 **Priority** field.

Key Contrasts**

- **Fragmentation:** IPv4 includes an **Identifier, Flags, and Fragment Offset** to handle packet fragmentation at routers. IPv6 **removed fragmentation** from the main header to speed up processing.
- **Checksum:** IPv4 has a **Header Checksum**; this was removed in IPv6 to reduce router overhead.
- **Options and Header Length:** IPv4 has a variable header length because of **Options**, requiring a **Header Length** field. IPv6 uses a fixed 40-byte header, making the length field unnecessary.
- **Flow Labeling:** IPv6 introduces a new 20-bit **Flow Label** to identify datagrams within a specific "flow," a concept not defined in the IPv4 header.
# question 9
 Suppose two packets arrive to two different input ports of a router at exactly the same time. Also suppose there are no other packets anywhere in the router
a.  Suppose the two packets are to be forwarded to two different output ports. Is it possible to forward the two packets through the switch fabric at the same time when the fabric uses a shared bus?
b.  Suppose the two packets are to be forwarded to two different output ports. Is it possible to forward the two packets through the switch fabric at the same time when the fabric uses switching via memory?
c.   Suppose the two packets are to be forwarded to the same output port. Is it possible to forward the two packets through the switch fabric at the same time when the fabric uses a crossbar? 

Based on the provided sources, here is the response to your query regarding router switching fabrics:

**a. Shared Bus with Different Output Ports** **No**, it is not possible to forward two packets through the switch fabric at the same time using a shared bus, even if they are destined for different output ports. In this architecture, datagrams are moved from input port memory to output port memory via a **shared bus**. Because the bus is shared, **bus contention** occurs, meaning only one packet can be transferred across the bus at a time. Consequently, the switching speed of the router is strictly limited by the bandwidth of this single bus.

**b. Switching via Memory with Different Output Ports** **No**, it is not possible to forward two packets simultaneously when switching via memory. In first-generation routers using this method, switching is under the direct control of the CPU, and packets are copied into the system’s memory. The switching speed is **limited by the memory bandwidth**, as each datagram requires **two bus crossings** (one to enter memory and one to leave it). Because these operations rely on the shared system bus and a single memory access point, packets must be processed sequentially.

**c. Crossbar with the Same Output Port** **No**, it is not possible to forward two packets to the same output port at the same time, even when using a crossbar or interconnection network. While a crossbar is designed to **exploit parallelism** by allowing multiple packets to be forwarded to _different_ output ports simultaneously, it cannot overcome **output port contention**. The sources specify that if two packets are destined for the same output port, only one can be transferred at a time, while the other must wait in a queue. This can lead to **Head-of-the-Line (HOL) blocking**, where a packet at the front of an input queue prevents others from moving forward because its destination port is currently busy.
# question 10
Suppose datagrams are limited to 1,500 bytes (including header) between source Host A and destination Host B. Assuming a 20-byte IP header, how many datagrams would be required to send an MP3 consisting of 5 million bytes? Explain how you computed your answer.

To determine how many datagrams are required to send the MP3 file, we must first identify the payload capacity of each datagram and then divide the total file size by that capacity.

Given Information**
- **Total file size (MP3):** 5,000,000 bytes.
- **Maximum datagram size:** 1,500 bytes (including the header).
- **IP header size:** 20 bytes.

Equation and Computation**
First, we calculate the amount of application data (payload) that can be carried in a single datagram: $$\text{Payload per Datagram} (P) = \text{Total Datagram Limit} - \text{IP Header Size}$$ $$P = 1,500 \text{ bytes} - 20 \text{ bytes} = 1,480 \text{ bytes}$$

Next, we calculate the total number of datagrams ($N$) required by dividing the total file size by the payload capacity: $$N = \frac{\text{Total File Size}}{\text{Payload per Datagram}}$$ $$N = \frac{5,000,000}{1,480} \approx 3,378.38$$

Since you cannot send a fraction of a datagram, you must round up to account for the remaining data. **Total datagrams required = 3,379**

Explanation of Computation**
1. **Determine Payload Capacity:** According to the sources, a network-layer datagram is formed by encapsulating transport-layer data with a network-layer header. Because the total size is limited to 1,500 bytes and the header consumes 20 bytes, only **1,480 bytes** of the actual MP3 file can fit into each packet.
2. **Fragmentation of Data:** In computer networking, large application messages must be broken into smaller chunks, known as packets or datagrams, to be transmitted across the network.
3. **Final Datagram Count:** By dividing the 5,000,000 bytes by the 1,480-byte capacity, we find that **3,378 datagrams** will be completely filled. The remaining 560 bytes (calculated as $5,000,000 \pmod{1,480}$) require a **3,379th datagram** to complete the transmission.
# question 11
Consider the network setup below (Figure 4.2). Suppose that the ISP instead assigns the router the address 24.34.112.235 and that the network address of the home network is 192.168.1/24.
a. Assign addresses to all interfaces in the home network.
b. Suppose each host has two ongoing TCP connections, all to port 80 at host 128.119.40.86. Provide the six corresponding entries in the NAT translation table.

Based on the network setup in Figure 4.2 of the sources, here are the address assignments and NAT translation table entries:

a. Address Assignments for Home Network Interfaces**

In a **/24 network**, the first 24 bits (the first three octets) represent the subnet, while the remaining bits identify individual hosts. For the network **192.168.1/24**, we can assign the following addresses to the interfaces:

- **Router (Internal/LAN Interface):** 192.168.1.4
- **Host 1:** 192.168.1.1
- **Host 2:** 192.168.1.2
- **Host 3:** 192.168.1.3

b. NAT Translation Table Entries**

A NAT translation table maps the **WAN side** (public IP and port) to the **LAN side** (private IP and port). According to the query, the router's public address is **24.34.112.235**, and each of the three hosts has two ongoing TCP connections to host **128.119.40.86** on port **80**.

Because each host is connecting to the same destination IP and port, the NAT router must assign a **unique public-side port number** to every individual connection to distinguish between them when responses arrive from the Internet.

The six corresponding entries in the NAT translation table would be:

|WAN side address|LAN side address|
|:--|:--|
|**24.34.112.235, 5001**|**192.168.1.1, 3345**|
|**24.34.112.235, 5002**|**192.168.1.1, 3346**|
|**24.34.112.235, 5003**|**192.168.1.2, 3345**|
|**24.34.112.235, 5004**|**192.168.1.2, 3346**|
|**24.34.112.235, 5005**|**192.168.1.3, 3345**|
|**24.34.112.235, 5006**|**192.168.1.3, 3346**|

_(Note: The port numbers 5001-5006 for the WAN side and 3345-3346 for the LAN side are examples based on the numbering patterns provided in Figure 4.2. In practice, any available port number above the well-known range could be used, provided the WAN side ports are unique.)_
# question 12
In this problem we’ll explore the impact of NATs on P2P applications. Suppose a peer with username Arnold discovers through querying that a peer with username Bernard has a file it wants to download. Also suppose that Bernard and Arnold are both behind a NAT. Try to devise a technique that will allow Arnold to establish a TCP connection with Bernard without application specific NAT configuration. If you have difficulty devising such a technique, discuss why.

To allow Arnold to establish a TCP connection with Bernard when both are behind a NAT, they can use a technique called **TCP Hole Punching** involving an intermediate signaling server.

The Technique

1. **Peer-to-Server Connection:** Both Arnold and Bernard first establish outgoing TCP connections to a publicly accessible intermediate server.
2. **Endpoint Discovery:** The server identifies the **public IP addresses and NAT-assigned port numbers** for both peers by looking at their NAT translation table entries.
3. **Signaling:** The server shares Arnold’s public IP/port with Bernard and vice versa.
4. **Simultaneous Initiation:** Both peers simultaneously attempt to initiate a TCP connection to the other's public endpoint.
    - When Arnold sends a TCP SYN packet, his NAT creates an entry (a "hole") expecting a response from Bernard.
    - Because Arnold’s NAT now has an entry for Bernard’s address, it will not drop Bernard’s incoming SYN packet, allowing the **three-way handshake** to complete.

Difficulties and Discussion

This technique is difficult to implement because **NATs violate the end-to-end argument** of the Internet. According to the sources, specific challenges include:

- **Port Manipulation:** NATs are controversial because they are network-layer devices that manipulate transport-layer port numbers.
- **Predictability:** Some NATs assign different ports for different destinations, making the port discovered by the intermediate server useless for direct peer-to-peer communication.
- **TCP Statefulness:** Standard TCP stacks often struggle with "simultaneous opens," and if the timing of the SYN packets is not precise, the NAT "holes" may not align correctly (Note: detailed TCP stack behavior regarding simultaneous opens is not fully detailed in the sources and should be verified independently).
- **Unsolicited Traffic:** By default, NATs are designed to drop all incoming traffic that does not match an internal request, essentially acting as a firewall for peers behind them.
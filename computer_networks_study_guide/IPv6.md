# IPv6

IPv6 is the successor to IPv4, designed primarily to address the exhaustion of the IPv4 address space and to simplify the IP header.

### Key Motivations for IPv6
- **Expanded Address Space:** The most critical driver. IPv6 uses **128-bit addresses**, providing a virtually inexhaustible number of unique IPs ($3.4 \times 10^{38}$ addresses).
- **Simplified Header:** The IPv6 header was redesigned to be simpler and more efficient for routers to process.

### IPv6 Datagram Format
The IPv6 header is a fixed length of **40 bytes**. Several fields from the IPv4 header were removed or moved to optional extension headers to speed up processing.

- **Key Fields:**
    - **Version:** 4 bits (value is 6).
    - **Traffic Class:** 8 bits (similar to the DiffServ field in IPv4).
    - **Flow Label:** 20 bits. A new field that can be used to identify packets belonging to the same flow, potentially allowing for special handling by routers.
    - **Payload Length:** 16 bits. The length of the data payload in bytes.
    - **Next Header:** 8 bits. Identifies the type of header immediately following the IPv6 header (e.g., a TCP header or an extension header).
    - **Hop Limit:** 8 bits (same as the TTL field in IPv4).
    - **Source and Destination Addresses:** 128 bits each.

- **Fields Removed from the IPv4 Header:**
    - **Header Length:** The IPv6 header is a fixed length.
    - **Fragmentation/Reassembly:** IPv6 routers do not perform fragmentation. If a packet is too large, the router drops it and sends an ICMPv6 "Packet Too Big" message back to the sender. Fragmentation, if needed, must be handled by the sending host.
    - **Header Checksum:** The checksum was removed to reduce processing time at each router. The link layer and transport layer are assumed to provide sufficient error detection.

### IPv6 Address Notation
IPv6 addresses are written as eight groups of four hexadecimal digits, separated by colons (e.g., `2001:0db8:85a3:0000:0000:8a2e:0370:7334`). Zeros can be compressed for brevity.

### Transition from IPv4 to IPv6
The transition from IPv4 to IPv6 has been slow and is still ongoing. Since not all routers can be upgraded simultaneously, a mechanism is needed for IPv4 and IPv6 networks to coexist. The primary mechanism is **tunneling**.

- **Tunneling:**
    - **Concept:** When an IPv6 datagram needs to travel through a region of the Internet that only supports IPv4 (an "IPv4 tunnel"), the entire IPv6 datagram is encapsulated inside an IPv4 datagram.
    - **Process:**
        1.  An IPv6-capable router at the entrance to the tunnel takes the IPv6 datagram and places it in the data field of a new IPv4 datagram.
        2.  This IPv4 datagram is addressed to the IPv6-capable router at the other end of the tunnel.
        3.  The IPv4 routers in the tunnel forward the packet based on its IPv4 header, oblivious to the fact that it contains an IPv6 datagram.
        4.  When the packet reaches the router at the end of the tunnel, the IPv4 header is stripped off, and the original IPv6 datagram is forwarded on to its destination.
- **Dual-Stack:** Many devices and networks today are "dual-stack," meaning they run both IPv4 and IPv6 simultaneously and can communicate using either protocol.

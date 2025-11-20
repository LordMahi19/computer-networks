# The Internet Protocol (IP)

The **Internet Protocol (IP)** is the network-layer protocol for the Internet. It defines the format of datagrams and provides the addressing scheme that allows devices to send and receive data across the global network.

### IPv4 Datagram Format
The IPv4 datagram is the fundamental unit of data at the network layer. Its header contains several important fields:

| Field                 | Size    | Description                                                                                             |
| --------------------- | ------- | ------------------------------------------------------------------------------------------------------- |
| **Version**           | 4 bits  | The IP protocol version (always 4 for IPv4).                                                            |
| **Header Length**     | 4 bits  | The length of the IP header in 32-bit words (typically 5, which means a 20-byte header).                |
| **Type of Service**   | 8 bits  | Used for Differentiated Services (DiffServ) to specify packet priority.                                 |
| **Total Length**      | 16 bits | The total length of the datagram (header + data) in bytes.                                              |
| **Identifier, Flags, Frag. Offset** | 32 bits | Fields used for **IP Fragmentation**. If a datagram is too large for a link it needs to cross, it can be broken into smaller fragments. These fields allow the destination host to reassemble the original datagram. |
| **Time to Live (TTL)**| 8 bits  | A counter that is decremented by 1 by every router that forwards the datagram. If the TTL reaches 0, the datagram is discarded. This prevents datagrams from looping forever in the network. |
| **Protocol**          | 8 bits  | Specifies the upper-layer protocol that should receive the data payload at the destination. (e.g., 6 for TCP, 17 for UDP). |
| **Header Checksum**   | 16 bits | A checksum calculated over the IP header only. It is used to detect errors in the header. It must be recomputed by every router because the TTL field changes at every hop. |
| **Source IP Address** | 32 bits | The 32-bit IP address of the sending host.                                                              |
| **Destination IP Address** | 32 bits | The 32-bit IP address of the receiving host.                                                          |
| **Options**           | variable| Optional fields, rarely used in practice.                                                             |
| **Data (Payload)**    | variable| The data from the transport layer (e.g., a TCP segment or UDP datagram).                                |

### IP Fragmentation and Reassembly
- **MTU (Maximum Transmission Unit):** Each link has an MTU, which is the maximum size of a frame it can carry.
- **Fragmentation:** If a router receives a datagram that is larger than the MTU of the outgoing link, it can fragment the datagram into two or more smaller IP datagrams.
- **Reassembly:** The fragments are reassembled into the original datagram only at the **final destination host**. Routers in the middle do not reassemble fragments.
- **Fields Used:** The `Identifier`, `Flags`, and `Fragment Offset` fields in the IP header are used to manage this process. All fragments of the same original datagram share the same identifier.
- **Drawbacks:** Fragmentation is computationally expensive and can be a security risk, which is one reason it was removed from the main header in IPv6.

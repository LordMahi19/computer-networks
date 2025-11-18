# Connectionless Transport: UDP

The **User Datagram Protocol (UDP)** is one of the two main transport protocols on the Internet. It provides a "no-frills," lightweight transport service.

### UDP Characteristics
- **"Best-Effort" Service:** UDP tries its best to deliver segments, but it makes no guarantees. Segments can be lost, delivered out of order, or contain errors.
- **Connectionless:** No "handshake" or connection setup is required before sending data. Each UDP segment is handled independently.
- **Minimal Overhead:** UDP adds very little overhead to the IP protocol.

### Advantages of UDP
1.  **No Connection Establishment Delay:** Since there is no three-way handshake, there is no RTT delay before data can be sent. This makes it ideal for delay-sensitive applications like DNS.
2.  **Simplicity:** There is no connection state (like receive buffers, congestion control parameters, etc.) to maintain at the sender or receiver.
3.  **Small Header Size:** The UDP header is only 8 bytes, compared to the 20-byte TCP header.
4.  **No Congestion Control:** UDP does not automatically throttle the sender in response to network congestion. This allows an application to send data at the desired rate, but it can also lead to high packet loss if not managed carefully by the application itself.

### When to Use UDP?
UDP is suitable for applications that are:
- **Loss-tolerant:** Can handle some data being lost without catastrophic failure (e.g., streaming video or audio, where a lost frame is a minor glitch).
- **Rate-sensitive:** Need to send data at a specific rate without being slowed down by congestion control.
- **Query-Response:** Simple, transactional applications where the overhead of setting up a TCP connection is unnecessary (e.g., DNS, SNMP).
- **Modern Protocols:** UDP is the foundation for modern protocols like **QUIC (HTTP/3)**, which implements reliability and congestion control in the application layer to overcome some of TCP's limitations (like head-of-line blocking).

### UDP Segment Header
The UDP header is simple and contains four fields:

| Field                 | Size    | Description                                                              |
| --------------------- | ------- | ------------------------------------------------------------------------ |
| **Source Port #**     | 16 bits | The port number of the sending application process.                      |
| **Destination Port #**| 16 bits | The port number of the receiving application process.                    |
| **Length**            | 16 bits | The length of the UDP segment (header + data) in bytes.                  |
| **Checksum**          | 16 bits | Used for error detection. It checks for bit errors in the segment.       |

### UDP Checksum
- **Purpose:** The checksum is used to detect "flipped bits" (errors) that may have occurred during the transmission of the segment.
- **How it works:**
    1.  The sender calculates the one's complement sum of the segment's contents (treated as a sequence of 16-bit integers). This includes the UDP header, the UDP data, and a "pseudo-header" containing the source and destination IP addresses.
    2.  The result is placed in the checksum field.
    3.  The receiver performs the same calculation on the received segment.
    4.  If the result is all '1's, it assumes no errors have occurred. If not, it knows the segment is corrupted and will typically discard it.
- **Note:** The checksum provides some protection, but it is not foolproof. It is also optional in IPv4 (but mandatory in IPv6).

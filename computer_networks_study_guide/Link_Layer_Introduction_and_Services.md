# Link Layer: Introduction and Services

The link layer is the second layer from the bottom in the Internet protocol stack. Its primary responsibility is to move a network-layer datagram from one node (a host or router) to an adjacent node over a single communication link.

### Terminology
- **Nodes:** The devices on a network, including hosts and routers.
- **Links:** The communication channels that connect adjacent nodes.
- **Frame:** The link-layer packet. A frame encapsulates a network-layer datagram.

**Analogy:** If a datagram is a tourist traveling from a source to a destination, the link layer is the specific mode of transportation for one leg of the journey (e.g., taking a bus from one city to the next). The datagram may be carried by different link-layer protocols on different links along its path (e.g., Ethernet, then WiFi, then a PPP link).

### Link Layer Services
The link layer provides several key services:

1.  **Framing:**
    - The link layer encapsulates the network-layer datagram in a **frame** by adding a header and sometimes a trailer.
    - The frame header contains information needed for the link-layer protocol, most importantly the source and destination **MAC addresses**.

2.  **Link Access:**
    - If the link is a shared medium (a **broadcast channel**), the link-layer protocol needs a **Multiple Access Protocol (MAC)** to coordinate access to the channel and manage collisions.

3.  **Reliable Delivery:**
    - Some link-layer protocols provide reliable delivery, ensuring that frames are transmitted across the link without error. This is typically done using acknowledgements and retransmissions.
    - This service is common on high-error-rate links like wireless, but is often considered an unnecessary overhead on low-error-rate links like fiber (where TCP's end-to-end reliability is sufficient).

4.  **Error Detection and Correction:**
    - **Error Detection:** The link layer includes error detection bits in the frame (e.g., a **CRC**) that allow the receiver to detect if bits were flipped during transmission. If an error is detected, the frame is typically dropped.
    - **Error Correction:** Some protocols allow the receiver to identify and correct bit errors without needing a retransmission.

5.  **Flow Control:**
    - Pacing the transmission rate between adjacent sending and receiving nodes to prevent the receiver's buffer from being overwhelmed.

### Where is the Link Layer Implemented?
The link layer is implemented in the **Network Interface Card (NIC)**, also known as the network adapter (e.g., an Ethernet card or a WiFi chip). It is a combination of hardware, software, and firmware.

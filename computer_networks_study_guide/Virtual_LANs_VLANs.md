# Virtual LANs (VLANs)

A standard switched LAN creates a single **broadcast domain**. This means that any layer-2 broadcast frame (like an ARP query or a DHCP discover message) is forwarded to every single host on the LAN. This presents several challenges as a network grows:
- **Lack of Traffic Isolation:** All hosts can see each other's broadcast traffic.
- **Inefficiency:** Every host must process every broadcast frame, wasting CPU cycles.
- **Security Concerns:** It's easier for malicious actors to observe network traffic.
- **Management Issues:** If a user moves their office, they may need to be physically re-cabled to stay on the same logical network.

**Virtual LANs (VLANs)** are a technology used to solve these problems by allowing a single physical LAN infrastructure to be partitioned into multiple logical, isolated broadcast domains.

### How VLANs Work
- **Port-Based VLANs:** The most common type. A network administrator configures the ports of a switch to be members of a specific VLAN.
- **Virtual Switches:** A single physical switch configured with VLANs acts like multiple separate virtual switches.
    - **Example:** On a 16-port switch, ports 1-8 could be assigned to VLAN 10 (e.g., for the Engineering department), and ports 9-16 could be assigned to VLAN 20 (e.g., for the Sales department).
- **Traffic Isolation:**
    - Traffic from a host in VLAN 10 can only reach other hosts in VLAN 10.
    - Broadcast frames sent by a host in VLAN 10 will only be forwarded to other ports in VLAN 10.
    - It is as if the Engineering and Sales departments are on two completely separate physical switches, even though they are using the same hardware.

### Forwarding Between VLANs
- VLANs create traffic separation at the link layer (Layer 2).
- To send traffic *between* two different VLANs (e.g., from a host in Engineering to a host in Sales), you need a **router** (a Layer 3 device).
- Typically, a router is connected to a port on the switch, and it is configured with interfaces on both VLAN 10 and VLAN 20. When the Engineering host wants to send a packet to the Sales host, it sends it to the router, which then forwards the packet to the Sales VLAN.

### VLANs Spanning Multiple Switches
- In a large network, a single VLAN may need to span multiple physical switches.
- A **trunk port** is a special port on a switch that is configured to carry traffic for *all* VLANs between the switches.
- **VLAN Tagging (802.1Q):** To distinguish which frames belong to which VLAN, frames sent over a trunk port are "tagged". The **IEEE 802.1Q** protocol adds an extra 4-byte **VLAN tag** to the Ethernet frame header.
    - This tag includes a **12-bit VLAN ID**, which uniquely identifies the VLAN.
    - When a tagged frame arrives at the next switch via the trunk port, the switch reads the VLAN ID and knows which VLAN the frame belongs to. It then removes the tag before forwarding the frame to a normal host port within that VLAN.

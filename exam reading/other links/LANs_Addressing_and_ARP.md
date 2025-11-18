# LANs: Addressing and ARP

In a Local Area Network (LAN), nodes need a way to identify each other at the link layer. This is done using MAC addresses, and the ARP protocol is used to link network-layer IP addresses to link-layer MAC addresses.

### MAC Addresses
- **Also known as:** LAN address, physical address, or Ethernet address.
- **Purpose:** To identify a specific network interface (NIC) on a local network segment. MAC addresses are used to forward frames from one interface to another *on the same subnet*.
- **Structure:** A **48-bit** address, typically written as six pairs of hexadecimal digits separated by hyphens or colons (e.g., `00-1A-2B-3C-4D-5E`).
- **Uniqueness:** MAC addresses are designed to be globally unique. The IEEE manages the address space. The first half of the address identifies the manufacturer, and the second half is a unique serial number assigned by that manufacturer.
- **Flat Address:** Unlike hierarchical IP addresses, MAC addresses are "flat". This provides portability, as a device can move from one LAN to another without changing its MAC address.
- **Broadcast Address:** The special MAC address `FF-FF-FF-FF-FF-FF` is the broadcast address. A frame sent to this address will be delivered to all nodes on the LAN.

### ARP (Address Resolution Protocol)
**The Problem:** A host or router knows the IP address of a device it wants to send a datagram to on the same subnet, but it needs to know the device's MAC address to actually build the link-layer frame.

**The Solution:** ARP translates IP addresses to MAC addresses.

- **ARP Table:** Each host and router on the LAN maintains an **ARP table** in its memory. This table contains mappings of IP addresses to MAC addresses for other nodes on the same subnet.
    - **Example Entry:** `< 192.168.1.100, 00-1A-2B-3C-4D-5E, 120 >`
    - **TTL (Time to Live):** Each entry has a TTL (typically 20 minutes) after which it is removed. This allows for changes in MAC addresses (e.g., if a NIC is replaced).

- **How ARP Works:**
    Let's say Host A (`192.168.1.100`) wants to send a datagram to Host B (`192.168.1.101`) on the same LAN.
    1.  Host A checks its ARP table for an entry for `192.168.1.101`.
    2.  **If an entry is found:** Host A uses the corresponding MAC address to create the Ethernet frame and sends it directly to Host B.
    3.  **If no entry is found (ARP miss):**
        a. Host A creates an **ARP query** packet. This packet contains the source IP/MAC (Host A) and the target IP (Host B), and asks "Who has IP address `192.168.1.101`?"
        b. Host A places this ARP query in an Ethernet frame with the destination MAC address set to the **broadcast address** (`FF-FF-FF-FF-FF-FF`).
        c. All nodes on the LAN receive and process the broadcast frame.
        d. Host B sees that the target IP in the ARP query matches its own IP address.
        e. Host B sends an **ARP response** packet directly back to Host A (unicast, not broadcast). This response contains its MAC address.
        f. Host A receives the ARP response, adds the new mapping to its ARP table, and can now send its original datagram to Host B.

### Sending a Datagram off the Subnet
- If Host A wants to send a datagram to a host on a different subnet (e.g., on the public Internet), it must send the frame to its **default gateway** (the local router).
- Host A will use ARP to find the MAC address of the router's interface on the LAN, and then send the frame (containing the IP datagram addressed to the final destination) to the router.
- **Key takeaway:** IP addresses remain constant from the original source to the final destination, but MAC addresses change at every hop (from host to router, from router to router, etc.).

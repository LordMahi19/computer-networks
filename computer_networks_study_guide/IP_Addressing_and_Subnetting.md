# IP Addressing and Subnetting

IP addressing is the foundation of the network layer, allowing every host and router on the Internet to have a unique identifier.

### IP Address and Interface
- **IP Address:** A 32-bit (for IPv4) number that uniquely identifies a device's connection to the network.
- **Interface:** The connection between a host/router and a physical link. Each interface has its own IP address. A router, by definition, has multiple interfaces.
- **Dotted-Decimal Notation:** IPv4 addresses are written in a human-readable format, with each of the four bytes represented as a decimal number separated by dots (e.g., `192.168.1.1`).

### Subnets
A subnet (or subnetwork) is a portion of a larger network. From a practical standpoint, a subnet consists of a group of device interfaces that can physically reach each other without passing through an intervening router.

- **Structure:** An IP address is divided into two parts:
    1.  **Subnet Part (Network Prefix):** The high-order bits of the address that are common to all devices on the same subnet.
    2.  **Host Part:** The low-order bits of the address that are unique to each device on the subnet.

- **Subnet Mask:** A 32-bit number that determines which part of the IP address is the subnet part and which is the host part.
- **CIDR (Classless InterDomain Routing):** This is the modern way of representing subnets. An address is written as `a.b.c.d/x`, where `x` is the number of bits in the subnet part.
    - **Example:** In the address `192.168.1.10/24`, the `/24` indicates that the first 24 bits (`192.168.1`) form the subnet part, and the remaining 8 bits (`10`) form the host part. All devices on the `192.168.1.0/24` subnet will have IP addresses from `192.168.1.1` to `192.168.1.254`.

### Forwarding and Longest Prefix Matching
- **Forwarding Table:** A router's forwarding table contains entries that map destination subnets to outgoing interfaces.
- **Route Aggregation:** CIDR allows for **route aggregation** (or summarization). An ISP can advertise a single, large address block (e.g., `200.23.16.0/20`) to the rest of the Internet, rather than advertising many smaller subnets within that block. This helps to keep the size of global routing tables manageable.
- **Longest Prefix Matching Rule:** When a router receives a datagram, it may find multiple entries in its forwarding table that match the destination address. The router will always use the entry with the **longest matching prefix**.
    - **Example:** If the table has entries for `200.23.16.0/20` and `200.23.20.0/23`, and a datagram arrives for `200.23.20.5`, the router will use the `/23` entry because it is a more specific match. This rule is essential for allowing more specific routes to override more general ones. This lookup is often performed using specialized, high-speed hardware called **Ternary Content Addressable Memories (TCAMs)**.

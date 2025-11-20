# The Transition to IPv6

The transition from IPv4 to IPv6 is a long-term global effort, driven by government regulations and the technical need for a larger address space.

### Global Push for IPv6
- Many governments around the world (including China, the USA, and India) have established aggressive timelines and mandates for their federal agencies and service providers to migrate to IPv6.
- This regulatory push is a major driver for adoption.

### Easy Steps for Adoption
- An enterprise network does not need to switch to being fully IPv6 overnight.
- **Dual-Stack:** The most common and preferable transition strategy is to run **dual-stack**, where devices and services are configured with both an IPv4 and an IPv6 address. Most modern applications, operating systems, and network devices support this.
- **Public-Facing Services:** A good first step is to enable IPv6 for public-facing websites and content. This is often easy to do by using a **Content Delivery Network (CDN)**, as major CDNs provide dual-stack options by default.
- **Happy Eyeballs (RFC 6555):** A technique used by modern clients (like web browsers) to improve user experience in dual-stack networks. The client tries to connect using both IPv6 and IPv4 simultaneously. Whichever connection is established first is the one that is used. This can often result in faster connection times, as seen in Vaisala's campus testing.

### IPv6 Transition Mechanisms
If a network is not fully dual-stack, mechanisms are needed to allow communication between IPv6-only and IPv4-only parts of the Internet.

- **Native IPv6 First:** The first priority should always be to use native IPv6 connectivity wherever possible.
- **NAT64:** This mechanism allows IPv6-only clients to connect to IPv4-only servers.
    - A **NAT64 gateway** sits at the boundary between the IPv6 and IPv4 networks.
    - When an IPv6 client wants to connect to an IPv4 server, it sends the packet to the NAT64 gateway.
    - The gateway synthesizes an IPv6 address that represents the IPv4 address and translates the IPv6 packet into an IPv4 packet, and vice-versa for the response.
- **DNS64:** A companion to NAT64. When an IPv6-only client makes a DNS query for a domain that only has an IPv4 (A) record, the DNS64 server synthesizes an IPv6 (AAAA) record and returns it to the client, directing it to the NAT64 gateway.
- **Tunneling:** An older mechanism where IPv6 packets are encapsulated inside IPv4 packets to cross IPv4-only parts of the network. Modern approaches like dual-stack and NAT64 are generally preferred.

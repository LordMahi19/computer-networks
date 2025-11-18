# Enterprise Network Design and Segmentation

Designing an enterprise network involves more than just connecting devices. It requires careful planning to ensure security, resilience, and functionality.

### Key Design Considerations
1.  **Analyze Needs:** Understand the current and future needs of the organization. A common problem is running out of IP addresses, so planning for growth is essential.
2.  **Security (The "Security Onion"):** Security must be considered in multiple layers (physical security, network security, host security).
3.  **Resilience and Recovery:** How will the network recover from problems?
    - What happens if there is a power failure?
    - What is the recovery plan if a construction worker cuts a fiber optic cable?
4.  **Traceability and Debugging:** How can network problems (e.g., a non-working node, incorrect routing) be diagnosed and fixed?

### Network Segmentation
A critical security practice in enterprise networks is **segmentation**: dividing the network into smaller, isolated zones to limit the impact of a security breach. If one segment is compromised, segmentation can prevent the attacker from easily moving to other parts of the network.

- **Simple Approach:** Use physically separate networks with routers in between.
    - **Problem:** This is expensive and requires a lot of extra cabling and hardware.

- **Modern Approach: VLANs:**
    - **Virtual LANs (VLANs)** are the primary tool for segmenting enterprise networks.
    - They allow a single physical network infrastructure to be logically divided into many separate virtual networks (e.g., a guest network, a corporate network, a manufacturing network, an R&D network).
    - VLANs operate at the link layer (Layer 2) and are separate from IP routing (Layer 3).
    - **VLAN trunks** are used to carry traffic for multiple VLANs over a single cable between switches.
    - **Security:** Switch configurations must be secured to prevent them from becoming an attack vector (e.g., an attacker trying to "jump" between VLANs).

### Example Corporate Segmentation
When deciding how to segment a corporate network, an administrator might create separate VLANs for different departments or functions, such as:
- Guests
- R&D
- Manufacturing
- Sales
- HR
- IT Services

The decision of what to segment is based on factors like the importance of the systems, the likelihood of them being a source of problems, and the type of equipment they need to connect to.

# Generalized Forwarding and SDN

Traditional forwarding decisions in routers are based solely on the destination IP address. However, a more powerful and flexible approach is **generalized forwarding**, which allows decisions to be based on a wider range of fields. This is the core concept behind **Software-Defined Networking (SDN)**.

### The "Match Plus Action" Abstraction
Generalized forwarding can be abstracted into a simple "match plus action" paradigm.
- **Flow:** A flow is a sequence of packets between a source and destination that share common header values (e.g., same source/dest IP, same source/dest port).
- **Flow Table:** A router (or switch) contains a flow table. Each entry in the flow table has three key components:
    1.  **Match (Pattern):** A set of values to match against header fields in arriving packets. This can include fields from the link, network, and transport layers (e.g., source MAC, dest IP, TCP port).
    2.  **Action:** An action to take on a packet that matches the pattern. Actions can include:
        - **Forward:** Forward the packet to a specific output port.
        - **Drop:** Discard the packet.
        - **Modify:** Modify a header field (e.g., as in NAT).
        - **Send to Controller:** Forward the packet to a central controller for processing.
    3.  **Priority & Counters:** A priority to resolve conflicts if a packet matches multiple rules, and counters to track the number of packets that have matched the rule.

### OpenFlow
**OpenFlow** is a specific protocol that implements the "match plus action" principle. It provides a standard way for a central controller to communicate with and manage the flow tables of network switches and routers.

### Software-Defined Networking (SDN)
SDN is a network architecture that decouples the network's control plane from its data plane.
- **Data Plane:** Consists of simple "match plus action" switches (often called OpenFlow switches). Their job is just to forward packets based on the flow tables they are given.
- **Control Plane:** A logically centralized **SDN controller** (or Network Operating System) is responsible for computing and installing the flow tables in the data plane switches. The controller has a global view of the network and can make intelligent routing and policy decisions.

**SDN Architecture:**
1.  **SDN-controlled Switches:** The hardware that forwards packets.
2.  **SDN Controller:** The "brain" of the network. It maintains network state and computes forwarding rules.
3.  **Network-Control Applications:** Applications that run on top of the controller and implement the network's logic (e.g., routing, firewalling, load balancing).

This architecture allows network administrators to programmatically control the entire network from a central point, making it much more flexible, agile, and easier to manage than traditional networks.

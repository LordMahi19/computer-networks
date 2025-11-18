# Network Layer Service Models

A network layer service model defines the level of service and the guarantees that the network provides for delivering datagrams between hosts.

### The Internet's "Best-Effort" Service Model
The Internet's network layer provides a single, simple service model: **best-effort service**.

This means the network makes its best effort to deliver datagrams, but it provides **no guarantees** whatsoever. Specifically, there are no guarantees on:
- **Guaranteed Delivery:** Datagrams may be lost.
- **Timing:** There is no guarantee that datagrams will arrive within a certain time (no maximum jitter).
- **Order:** Datagrams may arrive out of order.
- **Bandwidth:** There is no guarantee of a minimum available throughput.

**Why Best-Effort?**
- **Simplicity:** A simple, non-guaranteed service model makes the network core (routers) much simpler, faster, and cheaper.
- **Flexibility:** It provides a basic building block upon which more complex services can be built in the end systems (e.g., TCP builds a reliable service on top of the best-effort IP service).
- **Sufficient for Many Applications:** For "elastic" applications like email and file transfer, best-effort is sufficient. For real-time applications like voice and video, providing enough bandwidth ("over-provisioning") often results in performance that is "good enough".

### Alternative Service Models (Not in the Internet)
Other network architectures have proposed more complex service models that provide stronger guarantees. These are not widely used in the public Internet but are important concepts.

- **Constant Bit Rate (CBR):**
    - Used by ATM (Asynchronous Transfer Mode) networks.
    - Guarantees a constant, fixed amount of bandwidth for the duration of a connection.
    - Behaves like a dedicated, reserved circuit.

- **Variable Bit Rate (VBR):**
    - Also used by ATM.
    - Guarantees a minimum average bandwidth but allows the traffic to be "bursty".

- **IntServ (Integrated Services):**
    - An architecture that provides per-flow Quality of Service (QoS) guarantees in IP networks.
    - Applications can reserve resources (e.g., bandwidth, delay bounds) in routers along the path.
    - **Challenge:** Requires routers to maintain state for every single flow, which is not scalable for the public Internet.

- **DiffServ (Differentiated Services):**
    - A more scalable approach to QoS.
    - Instead of per-flow guarantees, it classifies packets into a small number of service classes.
    - Routers then apply different forwarding treatments (e.g., priority, different drop policies) based on the class of the packet.
    - Often used in private enterprise networks to prioritize traffic like Voice-over-IP (VoIP).

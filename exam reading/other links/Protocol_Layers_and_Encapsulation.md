# Protocol Layers and Encapsulation

To manage the complexity of computer networks, they are organized into a series of layers. This layered architecture provides a structured way to think about network components and allows for easier maintenance and updates.

### The Layered Internet Protocol Stack
The Internet protocol stack consists of five layers:

1.  **Application Layer:**
    - **Function:** Supports network applications and is where application-layer protocols reside.
    - **Examples:** HTTP (Web), SMTP (Email), DNS (Domain Name System).
    - **Data Unit:** Message.

2.  **Transport Layer:**
    - **Function:** Provides logical communication between processes on different hosts. It is responsible for process-to-process data transfer.
    - **Examples:** TCP (reliable, connection-oriented), UDP (unreliable, connectionless).
    - **Data Unit:** Segment.

3.  **Network Layer:**
    - **Function:** Responsible for routing datagrams from a source host to a destination host across one or more networks.
    - **Examples:** IP (Internet Protocol).
    - **Data Unit:** Datagram.

4.  **Link Layer:**
    - **Function:** Manages data transfer between two adjacent nodes (e.g., a host and a router, or two routers).
    - **Examples:** Ethernet, WiFi, PPP.
    - **Data Unit:** Frame.

5.  **Physical Layer:**
    - **Function:** Responsible for the actual transmission of bits ("on the wire") over the physical medium.
    - **Examples:** The physical properties of Ethernet cables, fiber optics, or radio waves.
    - **Data Unit:** Bit.

### Encapsulation
As data moves down the protocol stack from the application layer to the physical layer, each layer adds its own header information. This process is called **encapsulation**.

- **Process:**
    1.  The **Application Layer** creates a **message**.
    2.  The message is passed to the **Transport Layer**, which adds a transport header (e.g., TCP or UDP header) to create a **segment**.
    3.  The segment is passed to the **Network Layer**, which adds a network header (e.g., IP header) to create a **datagram**.
    4.  The datagram is passed to the **Link Layer**, which adds a link-layer header (e.g., Ethernet header) to create a **frame**.
    5.  The **Physical Layer** transmits the frame one bit at a time.

- **Analogy:** Encapsulation is like putting a letter (the message) into an envelope (the segment), then putting that envelope into a larger package (the datagram), and finally putting a shipping label on that package (the frame).

At the receiving end, the process is reversed (**decapsulation**), with each layer stripping off its corresponding header and passing the data up to the next layer until the original message reaches the receiving application.

### ISO/OSI Reference Model
Another important model is the seven-layer **Open Systems Interconnection (OSI) model**. While the five-layer Internet stack is what is actually implemented, the OSI model is often used as a theoretical reference. It includes two additional layers between the Application and Transport layers:
- **Presentation Layer:** Handles data formatting, encryption, and compression.
- **Session Layer:** Manages sessions between applications, including synchronization and recovery.

In the Internet model, these functions, if needed, must be implemented within the application layer.

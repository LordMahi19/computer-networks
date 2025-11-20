# Internet Overview

The Internet can be viewed from two perspectives:

### 1. "Nuts and Bolts" View
This view focuses on the physical and logical components that make up the Internet.

- **Hosts or End Systems:** Billions of connected computing devices (e.g., PCs, servers, smartphones, IoT devices) that run network applications at the Internet's "edge".
- **Packet Switches:** Devices that forward packets (chunks of data) toward their destination. The two main types are:
    - **Routers:** Used in the network core.
    - **Switches:** Typically used in the network edge (e.g., in a home or enterprise network).
- **Communication Links:** The physical media that connect devices.
    - **Transmission Rate (Bandwidth):** The rate at which data can be sent over a link, measured in bits per second (bps).
    - **Examples:** Fiber optic, copper wire, radio (WiFi, 4G/5G), satellite.
- **Networks:** Collections of devices, routers, and links managed by an organization. The Internet is a **"network of networks,"** a vast collection of interconnected Internet Service Providers (ISPs).

### 2. "Services" View
This view focuses on the infrastructure and programming interfaces that the Internet provides to applications.

- **Services for Applications:** The Internet provides services for a vast range of applications, including the Web, streaming video, email, online gaming, e-commerce, and social media.
- **Programming Interface ("Hooks"):** It offers a standardized interface for distributed applications. An application can send data by instructing the network to "send this message to that destination," similar to how the postal service works, without needing to know the details of the underlying network infrastructure.

### What is a Protocol?
Protocols define the rules that govern communication between entities in a network.

- **Human Analogy:** Just like humans have protocols for interaction (e.g., saying "hello" to initiate a conversation), network devices use protocols to communicate.
- **Network Protocols:** They define the **format** and **order** of messages sent and received, as well as the **actions** to be taken upon message transmission or receipt.
- **Examples:** HTTP (Web), TCP/IP, WiFi, Ethernet.
- **Standards Bodies:** Internet standards are developed by the **Internet Engineering Task Force (IETF)** and published as **Request for Comments (RFCs)**.

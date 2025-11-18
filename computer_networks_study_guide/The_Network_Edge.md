# The Network Edge

The network edge is where applications and hosts that use the network reside. It consists of end systems (hosts), access networks, and physical media.

### End Systems (Hosts)
- **Clients and Servers:** End systems are often categorized into clients and servers.
    - **Clients:** Devices that request services (e.g., a web browser on your laptop).
    - **Servers:** Powerful machines that provide services, often housed in large **data centers**.

### Access Networks
Access networks connect end systems to the first router (the "edge router") on the path to the Internet.

- **Digital Subscriber Line (DSL):**
    - Uses existing telephone lines.
    - Data and voice are transmitted at different frequencies.
    - Provides a **dedicated** line to the central office, so bandwidth is not shared with neighbors.
    - Asymmetric speeds: typically faster download (downstream) than upload (upstream).

- **Cable-Based Access (HFC):**
    - Uses the cable TV infrastructure (Hybrid Fiber Coax).
    - Data is transmitted on specific frequency channels.
    - It is a **shared** medium; bandwidth is shared among neighbors in the same cable network segment. This can lead to performance degradation if many users are active simultaneously.

- **Home Networks:**
    - Typically combine a DSL or cable modem with a router, firewall, and Network Address Translation (NAT).
    - Use Ethernet for wired connections and a WiFi access point for wireless connectivity.

- **Enterprise Networks:**
    - Used by companies and universities.
    - Employ a mix of wired (Ethernet) and wireless (WiFi) technologies with switches and routers to connect numerous devices.

- **Wireless Access Networks:**
    - **Wireless LANs (WLANs):** In a building or limited area (e.g., WiFi - IEEE 802.11). End systems connect to an Access Point (AP).
    - **Wide-Area Cellular Networks:** Over large geographic areas (e.g., 4G, 5G).

### Physical Media
Physical media are the actual links that transmit signals.

- **Guided Media:** Signals propagate in solid media.
    - **Twisted-Pair (TP) Copper Wire:** Used in Ethernet (e.g., Category 5, 6).
    - **Coaxial Cable:** Two concentric copper conductors.
    - **Fiber Optic Cable:** Transmits light pulses through glass fibers. Offers very high bandwidth, low error rates, and immunity to electromagnetic noise.

- **Unguided Media:** Signals propagate freely in the environment.
    - **Wireless Radio:** Carries signals in the electromagnetic spectrum.
    - Subject to environmental effects like reflection, obstruction, and interference.
    - Examples: WiFi, 4G/5G, Bluetooth, satellite radio.

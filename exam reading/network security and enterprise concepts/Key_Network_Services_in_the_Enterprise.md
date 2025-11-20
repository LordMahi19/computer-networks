# Key Network Services in the Enterprise

An enterprise network relies on a wide range of critical services to function correctly and securely.

### Most Important Services

- **Network Address Resolution:**
    - **DNS (Domain Name System):** Translates human-readable names to IP addresses.
    - **ARP (Address Resolution Protocol):** Translates IP addresses to MAC addresses on the local network.

- **Network Support Services:**
    - **DHCP (Dynamic Host Configuration Protocol):** Automatically assigns IP addresses to devices.
    - **ICMP (Internet Control Message Protocol):** Used for network diagnostics (e.g., `ping`).
    - **NAT (Network Address Translation):** Allows multiple devices to share a single public IP address.
    - **Routing Protocols:** (e.g., OSPF, BGP) Used by routers to learn paths through the network.
    - **VPN (Virtual Private Network):** Provides secure remote access to the corporate network.

- **Network Security Services:**
    - **Firewalls:** Filter traffic based on security rules.
    - **IDS (Intrusion Detection Systems):** Monitor for and alert on malicious activity.
    - **Device Authentication (802.1X):** Requires devices to authenticate before they are allowed to connect to the network.

- **User Authentication:**
    - **Active Directory / LDAP:** Centralized directory services for managing user accounts and permissions.
    - **Radius:** A protocol for centralized authentication, authorization, and accounting.
    - **MFA (Multi-Factor Authentication):** Requires users to provide multiple forms of verification to prove their identity.

- **Time Service (NTP):**
    - **Importance:** Accurate time is critical for many network functions, especially security. For example, a computer needs to know the correct time to verify the validity period of a digital certificate used for an HTTPS connection.
    - **Protocol:** **NTP (Network Time Protocol)** is used to synchronize the clocks of all devices on the network.
    - **Sources:** An enterprise needs a reputable source for time, such as a dedicated **GNSS** (GPS) appliance, or reliable public NTP servers (e.g., `time.google.com`). It's important to have backup time sources.

- **Split DNS:**
    - **Concept:** An enterprise often uses a "split DNS" configuration, where there are two separate DNS servers: one for the public Internet and one for the internal network.
    - **Purpose:**
        - To **hide the internal network structure** from the public. The public DNS server only contains records for public-facing servers (like the company's website). The internal DNS server contains records for all internal hosts and services.
        - To allow for easy-to-use domain names for internal services (e.g., `intranet.mycompany.com`).
    - **Corporate PKI:** To secure these internal services with HTTPS, the company needs its own internal **Public Key Infrastructure (PKI)** to issue digital certificates for its internal domain names.

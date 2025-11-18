# Network Security Fundamentals

The Internet was not originally designed with security as a primary concern. As a result, security must be considered at all layers of the protocol stack.

### Common Threats and Attacks

- **Packet Sniffing (Interception):**
    - Attackers can capture and inspect packets as they travel across a network, particularly on broadcast media like unencrypted WiFi.
    - This allows them to read sensitive information like passwords or personal data.
    - **Defense:** Encryption (confidentiality).

- **IP Spoofing (Fake Identity):**
    - An attacker can create an IP packet with a false source address to impersonate another device.
    - This can be used to bypass security controls that are based on IP addresses.
    - **Defense:** Authentication.

- **Denial of Service (DoS) Attacks:**
    - The goal of a DoS attack is to make a network resource (like a server or a link) unavailable to its legitimate users.
    - This is typically done by overwhelming the resource with a flood of bogus traffic, consuming its bandwidth or processing capacity.
    - A **Distributed Denial of Service (DDoS)** attack uses many compromised computers (a "botnet") to launch the attack, making it much harder to defend against.
    - **Defense:** Firewalls and Intrusion Detection Systems (IDS) that can filter malicious traffic.

### Lines of Defense

- **Authentication:**
    - **Goal:** To verify the identity of a user or device. "Are you who you say you are?"
    - **Methods:** Passwords, digital certificates, multi-factor authentication (MFA).

- **Confidentiality:**
    - **Goal:** To ensure that only the sender and intended receiver can understand the contents of a message.
    - **Method:** **Encryption**. Data is scrambled using a key, and only someone with the corresponding key can decrypt and read it.

- **Integrity:**
    - **Goal:** To ensure that a message has not been altered or tampered with during transit.
    - **Method:** **Digital signatures** and checksums. The receiver can verify the signature to confirm the message's integrity.

- **Access Control:**
    - **Goal:** To restrict access to network resources.
    - **Method:** **Firewalls**. These are specialized devices (or software) that sit at the boundary of a network and filter incoming and outgoing packets based on a set of security rules. They are a primary defense against DoS attacks and unauthorized access.

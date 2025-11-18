# The Domain Name System (DNS)

Humans prefer to use memorable, human-readable names for hosts (e.g., `www.google.com`), but the Internet's routing infrastructure uses 32-bit (IPv4) or 128-bit (IPv6) IP addresses. The **Domain Name System (DNS)** is the critical application-layer protocol that translates hostnames to IP addresses.

### DNS Services
- **Hostname to IP Address Translation:** The primary function of DNS.
- **Host Aliasing:** A host with a complex canonical name can have one or more aliases. DNS can be used to find the canonical name and IP address for an alias.
- **Mail Server Aliasing:** DNS `MX` records are used to find the name of the mail server for a particular domain.
- **Load Distribution:** A busy website may be replicated across multiple servers, each with its own IP address. DNS can return a set of IP addresses for a single hostname, allowing traffic to be distributed among the servers.

### DNS Architecture
DNS is a **distributed database** implemented in a **hierarchy** of name servers.

1.  **Root DNS Servers:** There are a small number of root servers worldwide that are the first point of contact for resolving unknown names. They know the addresses of the TLD servers.
2.  **Top-Level Domain (TLD) Servers:** These servers are responsible for top-level domains like `.com`, `.org`, `.net`, `.edu`, and all country-level domains like `.uk`, `.fr`, `.fi`. They know the addresses of the authoritative servers for their domains.
3.  **Authoritative DNS Servers:** Every organization with hosts on the Internet must provide publicly accessible authoritative DNS servers that hold the DNS records for their hosts.
4.  **Local DNS Name Servers:** While not strictly part of the hierarchy, local DNS servers are central to the architecture. When a host makes a DNS query, it is sent to its local DNS server (e.g., one provided by your ISP or university). The local server acts as a proxy, forwarding the query into the DNS hierarchy. It also **caches** recent DNS responses to improve performance.

### DNS Query Process
There are two main types of queries:

-   **Iterated Query:** This is the standard process. The local DNS server asks the root server, which replies with the address of the appropriate TLD server. The local server then asks the TLD server, which replies with the address of the authoritative server. Finally, the local server asks the authoritative server, which provides the IP address.
-   **Recursive Query:** The local DNS server asks the root server to perform the full resolution on its behalf. The root server then queries the TLD server, which queries the authoritative server. The final answer is passed back up the chain. This is less common in practice.

**Example (Iterated):**
1.  Your host asks your local DNS server: "What is the IP for `www.google.com`?"
2.  Local server asks a root server: "Where can I find `.com`?" Root server replies with TLD server addresses.
3.  Local server asks a TLD server: "Where can I find `google.com`?" TLD server replies with Google's authoritative server addresses.
4.  Local server asks Google's authoritative server: "What is the IP for `www.google.com`?" Authoritative server replies with the IP address.
5.  Local server sends the IP address to your host and caches the result.

### DNS Records
The DNS database stores **resource records (RRs)** in the format `(name, value, type, ttl)`.

- **TTL (Time to Live):** The amount of time a record can be cached before it should be discarded.
- **Common Record Types:**
    - **A (Address):** Maps a hostname to an IPv4 address. `(name=host.com, value=1.2.3.4, type=A)`
    - **AAAA (Address):** Maps a hostname to an IPv6 address.
    - **CNAME (Canonical Name):** Maps an alias name to a canonical (true) name. `(name=www.example.com, value=server1.example.com, type=CNAME)`
    - **MX (Mail Exchanger):** Specifies the name of the mail server for a domain. `(name=example.com, value=mail.example.com, type=MX)`
    - **NS (Name Server):** Specifies an authoritative name server for a domain.

### DNS Security
DNS is a critical but vulnerable part of the Internet infrastructure.
- **DDoS Attacks:** Attackers can try to overwhelm root or TLD servers with traffic.
- **Spoofing / Cache Poisoning:** An attacker can intercept a DNS query and send a fake response, causing a user to be directed to a malicious site. **DNSSEC (DNS Security Extensions)** is a protocol that provides authentication for DNS responses to prevent this.

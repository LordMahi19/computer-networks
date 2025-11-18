The source material for **"slide 6.pdf"** outlines a presentation on **Computer Networks in an Enterprise Environment**, given by Vesa Jääskeläinen of Vaisala Oyj. The topics focus heavily on practical enterprise networking concerns, including modeling, segmentation, security, network services, virtualization, and the transition to IPv6.

Here are all the categories and topics mentioned in "slide 6.pdf":

### I. Introduction and Overview

- **Speaker/Company:** Vesa Jääskeläinen (Principal Software Architect, R&D) from Vaisala Oyj.
- **Speaker Background:** Master of Science from Lappeenranta University of Technology (2006) in IT/Software Engineering. Work includes Vaisala's Embedded Linux Platform, helping other teams, security, manufacturing, and R&D <-> IT co-operation.
- **Main Topics (Roadmap):** Who am I?, Vaisala, Networks, Network Services, IPv6, and Vaisala Opportunities.

### II. Networks (Design and Implementation)

- **Modelling Networks (Common Steps):**
    - **Analyze the needs** now and a bit further in the future.
    - Address the **common problem** of running out of IP addresses in a network.
    - Consider **network security and physical security**, often referred to as the **security onion model** (protections in multiple layers).
    - Think about **network functionality in special situations** (e.g., campus power failure).
    - Think about **network resiliency** (i.e., how to recover from problems, such as a construction worker breaking an optical fiber cable).
    - Think about **network traceability** (i.e., how to debug problems like a non-working network node or incorrect routing).
- **Network Segmentation (Slicing Networks):**
    - **Simplest way:** Use physically different networks with routers in between.
    - **Problem with simplest approach:** Requires many "extra" cables and network switches.
    - **Solution: Virtual LANs (VLANs):** Developed to construct **virtual physical networks**.
        - VLANs work in the **data link layer (Ethernet)** and are separate from the network layer's (IP) routing.
        - Requires a coordinated list of VLANs in campus.
        - VLAN trunks create a **shared "physical networks" medium** in one cable.
        - Network switch configuration requires added **security controls** to prevent it from becoming an attack vector.
- **Corporate Environment Example:** Discussing how to segment a network based on functions like Communications & PR, Guests, HR, IT services, Legal, Manufacturing, Other business support, R&D, and Sales, considering factors like importance, likely problem sources, and necessary equipment.
- **Internet Protocol Usage:** Today's traffic mostly uses **Internet Protocol v4** and **Internet Protocol v6**.
    - Transfer mediums are **Ethernet and WIFI**.
    - **VLAN** is the main Ethernet level protocol for campus segmentation.
    - The majority of traffic uses **TCP** and, on top of that, **HTTPS**.

### III. Network Services

- **Most Important Services:**
    - **Network address resolution:** DNS, ARP, etc..
    - **Network support services:** DHCP, ICMP, NAT, Routing, VPN, etc..
    - **Network security services:** Device Authentication, Intrusion Detection Systems, Network Firewalls, etc..
    - **User authentication:** Active Directory, Digital Certificates, IdP, MFA, Radius, etc..
    - **Time:** NTP, SNTP, etc..
    - **HTTPS** (websites) and **SMTP** (e-mail).
- **Time Service:**
    - Time is very important for computers.
    - **NTP/SNTP** are primary sources of time, but the security of time delivery needs to be understood, as secure protocol standardization is still pending.
    - Necessary for verifying **Digital certificates** to establish HTTPS/TLS connections.
    - Requires a reputable source for time, possibly **GNSS** (Global Navigation Satellite System) for local, accurate time.
    - Backup time service(s) are recommended for maintenance or partial network outages.
    - Other sources include national time sources, ntppool.org, time.aws.com, time.cloudflare.com, and time.google.com.
- **Split DNS:**
    - Involves different views of DNS from the public network and the internal network.
    - Used to direct traffic to different places and mainly to **hide internal networks from the public**.
    - Allows for easy domain names for internal services, which requires **corporate PKI systems** to support HTTPS/TLS.
- **Virtualization Environments:**
    - **Broadcom’s (VMWare) vSphere** is noted as the market leader.
    - Cloud vendors provide virtual machines, but caution is advised regarding their pricing.
    - **Kubernetes** is well known but **hard to maintain**, suggesting the use of commercial offerings with support agreements (Managed Kubernetes in the cloud is a good solution).
    - Most enterprises use **mixed cloud – on-prem environments**.
    - The modern mantra is to implement features where they are best implemented and utilize cloud features where they provide the most benefit.
    - Some systems are more practical to implement on-prem due to price, functionality, or special situations.

### IV. Support and Maintenance

- **Development vs. Commercial Solution:** The work required to develop a custom system must not be underestimated compared to buying a commercial solution.
- **Consequences of Failure:** If critical systems fail, the cost is counted in seconds.
    - Public examples mentioned include Nordea having banking service problems and CrowdStrike’s security software update causing global system failure and halting part of the air traffic.
- **Recovery plans** are a well-spent activity, even if never used.

### V. IPv6 Transition and Adoption

- **Governments and Regulations:** Details on international regulatory push for IPv6 adoption:
    - **China (2021 plan):** Increase IPv6 traffic share to 50% by end of 2023, 70% traffic share and 800 million IPv6 addresses by end of 2025, and completely phase out IPv4 around 2030.
    - **Finland:** Recommendation to adapt IPv6 in consumer networks.
    - **Norway:** IPv6 supported, dual-stack possibility.
    - **Sweden:** Recommendation for IPv6, no requirement.
    - **India:** Government organizations to complete IPv6 migration by mid-2022; new retail wireline customer connections must be IPv6 capable by end of 2022; Service Providers to replace non-IPv6 ready CPEs by end of 2022.
    - **Malaysia:** IPv6 test Reports will be mandatory for approval/renewal of non-Wi-Fi-enabled devices by July 2025.
    - **USA (Whitehouse Executive Order M-21-07):** Update Federal information systems to native IPv6 operation, with milestones: 20% of IP-enabled assets in IPv6-only environments by end of FY 2023, 50% by end of FY 2024, and 80% by end of FY 2025.
- **Guidance in Procurement Process (RIPE NCC):** Requirement for system integrators to have valid certificates and general knowledge of IPv6 protocol, planning, and security, with cancellation possible if competence is insufficient during installation.
- **IPv6 Testing:** Mentions tests for DNA, Elisa, Telia, Global organizations, Finnish networks, companies, universities, and travel-related entities.
- **Easy Steps to Do:**
    - No need for corporate networks to be fully IPv6 immediately (a big undertaking).
    - If starting fresh, consider IPv6 right away.
    - Use cloud services and **Content Delivery Networks (CDN)** for public corporate websites/content, as they come with **dual-stack options**.
    - Most applications and network devices/services support **dual-stack**.
    - Most AWS services are IPv6 ready.
- **Vaisala Campus Testing:**
    - Planned to bring IPv6 for product testing purposes into Vantaa campus.
    - Initial IPv6 trainings were held years ago; funding approved for 2025.
    - First connectivity tests were done in summer 2025 using a network configuration expert.
    - Initial test points provided for R&D with basic services.
    - **Unexpected benefit:** Web browsing got faster, likely due to RFC 6555 – **Happy Eyeballs**.
- **IPv6 Transition Methods:**
    - Many older transition mechanisms are obsolete.
    - First priority should be using **native IPv6** (e.g., **dual-stack**).
    - If IPv6 $\rightarrow$ IPv4 access is needed, **NAT64** can be utilized, with the well-known prefix `64:ff9b::/96`.
    - NAT64 translation shorthand is available (`<nat64-prefix>:<ipv4-address>`).
    - NAT64 can be paired with DNS records or **DNS64** for automated translation generation.
    - Recommendation is to fix things to be native IPv6/dual-stack, possibly by disabling IPv4 in a laptop to test.
    - **Dual stacking your entire network is the preferable solution if possible**.

### VI. Vaisala Opportunities (Career)

- **Company Metrics:** 300+ product families, 59 nationalities, 2,400+ experts, 28% of people work in R&D, 150 countries, 1,400+ shipments weekly, 565 MEUR net sales.
- **Employee Focus:** Scores 4.0/5 in employee engagement and has zero gender-pay gap. Provides career growth through learning, mentorship, and international job rotations.
- **R&D Environment:** 28% work in R&D, utilizing advanced laboratories and in-house cleanrooms.
- **Internship:** Launches about 25 new projects for students annually; the application period starts in January 2026 for the internship period May/June – August/September (Giant Leap Internship Program).
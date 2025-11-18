# Virtualization and Cloud in the Enterprise

Modern enterprise IT infrastructure is heavily reliant on virtualization and a mix of on-premises and cloud-based resources.

### Virtualization Environments
- **Concept:** Virtualization allows a single physical server to run multiple independent virtual machines (VMs). Each VM has its own operating system and applications and is isolated from the others.
- **Market Leader:** **VMware vSphere** is a dominant platform for on-premises server virtualization.
- **Benefits:** Increased server utilization, easier management, and faster deployment of new services.

### Cloud Environments
- **Cloud Providers:** Major cloud vendors (like Amazon Web Services, Microsoft Azure, Google Cloud) provide virtual machines and a vast array of other services on demand.
- **Kubernetes:** A popular open-source platform for managing containerized applications. It is very powerful but can be complex to maintain, so many enterprises use managed Kubernetes offerings from cloud providers.

### The Hybrid Model: Mixed Cloud and On-Premises
Most enterprises today use a **hybrid model**, combining their own on-premises data centers with services from public cloud providers.

- **The Modern Mantra:** Implement features and services where they are best suited.
    - Utilize cloud features where they provide the most benefit (e.g., for massive scalability, global reach, or specialized services like machine learning).
    - Keep some systems on-premises for reasons of cost, security, performance (low latency), or because they connect to specialized physical equipment (e.g., in a manufacturing or R&D environment).

### Build vs. Buy: Development vs. Commercial Solutions
- A critical decision for any enterprise is whether to build a custom system or buy a commercial, off-the-shelf solution.
- The amount of work required to develop, maintain, and support a custom system should not be underestimated.
- **Consequences of Failure:** For critical systems, failure can be extremely costly. Public examples like major banking service outages or security software updates causing global system failures highlight the importance of reliability.
- **Recovery Plans:** Having well-thought-out recovery plans is a crucial activity, even if they are never used.

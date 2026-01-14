The sources from the first slide deck mention several mathematical formulas and expressions used to calculate network performance and scaling:

- **Packet Transmission Delay ($d_{trans}$)**: This is the time required to push all of the packet's bits into the link, calculated as **$L / R$**, where $L$ is the packet length in bits and $R$ is the link transmission rate in bits per second.
- **Total Nodal Delay ($d_{nodal}$)**: The total delay at a node is the sum of four sub-delays: **$d_{nodal} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$**.
- **Propagation Delay ($d_{prop}$)**: This is the time it takes for a bit to travel from the start of the link to the next node, calculated as **$d / s$**, where $d$ is the length of the physical link and $s$ is the propagation speed of the medium (approximately $2 \times 10^8$ m/sec).
- **Traffic Intensity**: This value is used to estimate queuing delay and is defined as **$La / R$**, where $L$ is the packet length, $a$ is the average packet arrival rate, and $R$ is the transmission rate. The sources note that as traffic intensity approaches 1, the average queuing delay becomes very large.
- **Per-connection Throughput**: In a network scenario where 10 connections share a bottleneck backbone link, the throughput is calculated as **$min(R_c, R_s, R/10)$**, where $R_c$ is the client link capacity, $R_s$ is the server link capacity, and $R$ is the shared link capacity.
- **Network Scaling (Big O Notation)**: The sources state that connecting every access ISP directly to every other ISP does not scale because it requires **$O(N^2)$** connections.


Based on the mathematical principles provided in the sources, here are several practice questions and their answers to help you apply these formulas in a networking context.

### **1. Packet Transmission Delay ($d_{trans}$)**

**Question:** A host is sending a large data file divided into packets of **$L = 8,000$ bits** each. The host is connected to an access network with a **transmission rate ($R$) of 2 Mbps** (2,000,000 bits per second). What is the transmission delay for a single packet?

**Answer:**

- **Formula:** $d_{trans} = L / R$.
- **Calculation:** $8,000 \text{ bits} / 2,000,000 \text{ bps} = 0.004 \text{ seconds}$.
- **Result:** The transmission delay is **4 msec**.

---

### **2. Propagation Delay ($d_{prop}$)**

**Question:** Two routers are connected by a **fiber optic link** that is **$d = 1,000$ km** long. If the **propagation speed ($s$)** of the signal in the fiber is **$2 \times 10^8$ m/sec**, how long does it take for the first bit of a packet to travel between the routers?

**Answer:**

- **Formula:** $d_{prop} = d / s$.
- **Calculation:** $1,000,000 \text{ meters} / 200,000,000 \text{ m/sec} = 0.005 \text{ seconds}$.
- **Result:** The propagation delay is **5 msec**.

---

### **3. Traffic Intensity and Queuing Delay**

**Question:** A router receives packets of **length $L = 1,500$ bits** at an average **arrival rate ($a$) of 800 packets per second**. The outgoing link has a **transmission rate ($R$) of 1.5 Mbps**.

1. Calculate the **traffic intensity**.
2. Based on this value, will the **average queuing delay** be small, large, or infinite?

**Answer:**

- **Formula:** Traffic Intensity $= La / R$.
- **Calculation:** $(1,500 \text{ bits} \times 800 \text{ pkts/sec}) / 1,500,000 \text{ bps} = 1,200,000 / 1,500,000 = 0.8$.
- **Result:** The traffic intensity is **0.8**. Since the value is **approaching 1**, the average **queuing delay will be large**.

---

### **4. Total Nodal Delay ($d_{nodal}$)**

**Question:** A packet arrives at a router. The **processing delay ($d_{proc}$)** is **10 $\mu$sec**, the **queuing delay ($d_{queue}$)** is **2 msec**, the **transmission delay ($d_{trans}$)** is **1 msec**, and the **propagation delay ($d_{prop}$)** is **3 msec**. What is the total nodal delay?

**Answer:**

- **Formula:** $d_{nodal} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$.
- **Calculation:** $0.01 \text{ msec} + 2 \text{ msec} + 1 \text{ msec} + 3 \text{ msec} = 6.01 \text{ msec}$.
- **Result:** The total nodal delay is **6.01 msec**.

---

### **5. Per-connection Throughput**

**Question:** A server has a **link capacity ($R_s$) of 100 Mbps**, and a client has a **link capacity ($R_c$) of 20 Mbps**. They are communicating through a **shared backbone link with a capacity ($R$) of 150 Mbps**. If there are **10 simultaneous connections** sharing that backbone link equally, what is the expected throughput for the client-server connection?

**Answer:**

- **Formula:** Throughput $= min(R_s, R_c, R/N)$.
- **Calculation:** $min(100, 20, 150/10) = min(100, 20, 15)$.
- **Result:** The throughput is **15 Mbps** because the **shared backbone link is the bottleneck** ($R/10 = 15$).

---

### **6. Network Scaling (Big O Notation)**

**Question:** If an internet architect suggests connecting every one of the **$N$ access ISPs** directly to every other access ISP to eliminate the need for Tier-1 ISPs, why is this considered a failure in scaling?

**Answer:**

- **Concept:** Connecting $N$ nodes to every other node results in a **mesh topology**.
- **Formula:** The number of required connections is **$O(N^2)$**.
- **Result:** This does not scale because as the number of ISPs ($N$) reaches millions, the number of physical connections required becomes **mathematically unmanageable and economically unviable**.



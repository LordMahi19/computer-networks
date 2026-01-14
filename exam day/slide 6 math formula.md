Based on the sources provided for **slide 6.pdf**, there are **no traditional mathematical formulas** mentioned.

The content of "slide 6.pdf" focuses on computer networks in an enterprise environment, corporate network segmentation, and IPv6 transition regulations rather than theoretical networking calculations found in other provided sources. While slides from other files (such as "slide 1.pdf" or "slide 3.pdf") include formulas for transmission delay ($L/R$) or the TCP round-trip time ($EstimatedRTT = (1- \alpha) \cdot EstimatedRTT + \alpha \cdot SampleRTT$), the sources for "slide 6.pdf" contain only symbolic notations and quantitative indices:

- **NAT64 Shorthand Translation:** A notation used for IPv6 to IPv4 access represented as **`<nat64-prefix>:<ipv4-address>`**. An accompanying example shows that `64:ff9b::192.168.1.1` resolves to `64:ff9b::c0a8:101`.
- **Employee Engagement Index:** A quantitative metric expressed as a score of **4.0/5**.
- **Target IPv6 Deployment Percentages:** The sources mention goals for Federal information systems to reach **20%**, **50%**, and **80%** operating in IPv6-only environments by specific fiscal years.




Since the sources for **slide 6.pdf** primarily contain symbolic notation rather than complex calculations, I have compiled a set of practice questions and answers drawing on the core mathematical formulas found throughout the provided networking materials (specifically from slides 1, 2, 3, and 5) to help you master these concepts.

### 1. Packet Transmission Delay

**Formula:** $d_{trans} = L / R$

- **L:** Packet length in bits.
- **R:** Link transmission rate (bandwidth) in bits per second (bps).

**Question:** A host is sending a **10 Kbit** packet over a **100 Mbps** link. What is the one-hop transmission delay? **Answer:** Convert units: $L = 10,000$ bits; $R = 100,000,000$ bps. Calculation: $10,000 / 100,000,000 = 0.0001$ seconds or **0.1 msec**.

---

### 2. Traffic Intensity

**Formula:** $Traffic Intensity = La / R$

- **L:** Packet length (bits).
- **a:** Average packet arrival rate (packets/sec).
- **R:** Link bandwidth (bps).

**Question:** If the traffic intensity $La/R$ is greater than **1**, what happens to the average queueing delay? **Answer:** The average queueing delay becomes **infinite**, as "work" is arriving faster than the link can service it.

---

### 3. Non-persistent HTTP Response Time

**Formula:** $Response Time = 2 \cdot RTT + \text{file transmission time}$

- **RTT:** Round Trip Time for a small packet to travel from client to server and back.

**Question:** A client uses non-persistent HTTP to request a single file. If the RTT is **50 ms** and the file takes **20 ms** to transmit, what is the total response time? **Answer:** $2 \cdot (50\text{ ms}) + 20\text{ ms} = 100\text{ ms} + 20\text{ ms} =$ **120 ms**. _Note: This accounts for one RTT to initiate the TCP connection and one RTT for the HTTP request/response header._

---

### 4. TCP Estimated Round-Trip Time (EstimatedRTT)

**Formula:** $EstimatedRTT = (1 - \alpha) \cdot EstimatedRTT + \alpha \cdot SampleRTT$

- **Typical value:** $\alpha = 0.125$.

**Question:** The current $EstimatedRTT$ is **100 ms**. A new $SampleRTT$ arrives at **140 ms**. Using $\alpha = 0.125$, what is the new $EstimatedRTT$? **Answer:** $(1 - 0.125) \cdot 100 + (0.125 \cdot 140) = (0.875 \cdot 100) + 17.5 = 87.5 + 17.5 =$ **105 ms**.

---

### 5. CSMA/CD Efficiency

**Formula:** $Efficiency = 1 / (1 + 5 \cdot d_{prop} / d_{trans})$

- **$d_{prop}$:** Max propagation delay.
- **$d_{trans}$:** Time to transmit a max-size frame.

**Question:** What happens to the efficiency of CSMA/CD as the propagation delay ($d_{prop}$) approaches **0**? **Answer:** The efficiency goes to **1** (or 100%), meaning the protocol becomes more effective at utilizing the channel.

---

### 6. NAT64 Shorthand Translation (from Slide 6)

**Notation:** `<nat64-prefix>:<ipv4-address>`

**Question:** Using the well-known prefix **64:ff9b::/96**, translate the IPv4 address **192.168.1.1** into its NAT64 representation. **Answer:** The shorthand resolves to **64:ff9b::192.168.1.1**. _Insight from outside sources: In hexadecimal, this translates specifically to 64:ff9b::c0a8:101._

---

### 7. Throughput in a Shared Network

**Formula:** $\min(R_s, R_c, R/N)$

- **$R_s$:** Server link capacity.
- **$R_c$:** Client link capacity.
- **$R/N$:** Shared backbone bandwidth divided by $N$ connections.

**Question:** A server has an upload rate ($R_s$) of **10 Mbps** and the client has a download rate ($R_c$) of **2 Mbps**. They share a high-speed backbone that is not congested. What is the end-to-end throughput? **Answer:** The throughput is constrained by the **bottleneck link**, which is the minimum of the rates. $\min(10\text{ Mbps}, 2\text{ Mbps}) =$ **2 Mbps**.
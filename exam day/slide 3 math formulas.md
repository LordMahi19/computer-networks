The following mathematical formulas and expressions are mentioned in the sources derived from **slide 3.pdf**, which covers the Transport Layer:

### **Packet Transmission and Utilization**

- **Transmission Delay ($D_{trans}$):** The time needed to transmit a packet is calculated as $D_{trans} = L / R$, where $L$ is the packet length in bits and $R$ is the link transmission rate.
- **Stop-and-Wait Utilization ($U_{sender}$):** The fraction of time the sender is busy sending in a stop-and-wait protocol is expressed as: $$U_{sender} = \frac{L / R}{RTT + L / R}$$.
- **Pipelined Utilization:** For a protocol utilizing pipelining (specifically a 3-packet pipelining example), the utilization increases by the number of packets ($N$) in flight: $$U_{sender} = \frac{3 \cdot (L / R)}{RTT + L / R}$$.

### **TCP Round Trip Time (RTT) and Timeout Estimation**

- **Estimated RTT:** To smooth out variations in SampleRTT, TCP uses an Exponential Weighted Moving Average (EWMA): $$EstimatedRTT = (1- \alpha) \cdot EstimatedRTT + \alpha \cdot SampleRTT$$ The typical value for $\alpha$ is 0.125.
- **DevRTT:** This calculates the EWMA of the SampleRTT deviation from the EstimatedRTT: $$DevRTT = (1-\beta) \cdot DevRTT + \beta \cdot |SampleRTT - EstimatedRTT|$$ The typical value for $\beta$ is 0.25.
- **Timeout Interval:** The safety margin for the timeout interval is defined as: $$TimeoutInterval = EstimatedRTT + 4 \cdot DevRTT$$.

### **TCP Congestion Control and Rate**

- **Sender Transmission Constraint:** The sender limits the amount of unacknowledged data using the congestion window ($cwnd$): $$LastByteSent - LastByteAcked < cwnd$$.
- **TCP Rate Approximation:** The sending behavior of TCP is roughly estimated as: $$TCP \text{ rate} \approx \frac{cwnd}{RTT} \text{ bytes/sec}$$.
- **Congestion Window Adjustments:**
    - On a loss event, the slow start threshold ($ssthresh$) is set to half of the current $cwnd$: $ssthresh = cwnd / 2$.
    - During congestion avoidance, the window is updated per ACK as: $cwnd = cwnd + MSS \cdot (MSS / cwnd)$.
    - In fast recovery, the window may be adjusted as: $cwnd = ssthresh + 3$.

### **Throughput and Fairness**

- **Throughput (Delay-based TCP):**
    - **Uncongested Throughput:** Calculated as $cwnd / RTT_{min}$.
    - **Measured Throughput:** Calculated as $(\text{# bytes sent in last RTT interval}) / RTT_{measured}$.
- **Fairness Goal:** If $K$ TCP sessions share a bottleneck link of bandwidth $R$, the fair average rate for each should be: $$\text{Average rate} = R / K$$,.



To practice the mathematical formulas found in the sources, particularly from **slide 3.pdf**, here are several practice questions and their step-by-step solutions based on the transport layer concepts.

### **1. Packet Transmission Delay**

**Question:** A host needs to transmit a packet of **10 Kbits** over a link with a transmission rate of **100 Mbps**. What is the transmission delay ($D_{trans}$)?

**Answer:**

- **Formula:** $D_{trans} = L / R$.
- **Calculation:**
    - $L = 10,000$ bits.
    - $R = 100,000,000$ bits/sec.
    - $D_{trans} = 10,000 / 100,000,000 = 0.0001$ seconds.
- **Result:** The transmission delay is **0.1 msec**.

---

### **2. Sender Utilization (Stop-and-Wait vs. Pipelining)**

**Question:** Using the transmission delay ($L/R$) of **0.008 msec** and an RTT of **30 msec**, compare the utilization ($U_{sender}$) of a Stop-and-Wait protocol versus a pipelined protocol sending **3 packets** at once.

**Answer:**

- **Stop-and-Wait Formula:** $U_{sender} = \frac{L / R}{RTT + L / R}$.
    - $U_{sender} = 0.008 / (30 + 0.008) = 0.008 / 30.008 \approx \mathbf{0.00027}$.
- **Pipelining Formula (N=3):** $U_{sender} = \frac{3 \cdot (L / R)}{RTT + L / R}$.
    - $U_{sender} = (3 \cdot 0.008) / (30 + 0.008) = 0.024 / 30.008 \approx \mathbf{0.00081}$.
- **Insight:** Pipelining increases utilization by a factor of $N$ (in this case, 3).

---

### **3. TCP Round Trip Time (RTT) Estimation**

**Question:** A TCP connection has a current **EstimatedRTT of 100 msec**. It receives a new **SampleRTT of 120 msec**. Using the standard $\alpha = 0.125$, calculate the new EstimatedRTT.

**Answer:**

- **Formula:** $EstimatedRTT = (1- \alpha) \cdot EstimatedRTT + \alpha \cdot SampleRTT$.
- **Calculation:**
    - $New EstimatedRTT = (1 - 0.125) \cdot 100 + 0.125 \cdot 120$.
    - $New EstimatedRTT = (0.875 \cdot 100) + (0.125 \cdot 120)$.
    - $New EstimatedRTT = 87.5 + 15 = \mathbf{102.5 \text{ msec}}$.

---

### **4. Setting the Timeout Interval**

**Question:** If the **EstimatedRTT is 102.5 msec** and the current **DevRTT is 5 msec**, what should the TCP **TimeoutInterval** be?

**Answer:**

- **Formula:** $TimeoutInterval = EstimatedRTT + 4 \cdot DevRTT$.
- **Calculation:**
    - $TimeoutInterval = 102.5 + 4 \cdot 5$.
    - $TimeoutInterval = 102.5 + 20 = \mathbf{122.5 \text{ msec}}$.

---

### **5. Estimating TCP Sending Rate**

**Question:** A TCP sender has a congestion window ($cwnd$) of **64,000 bytes** and an RTT of **100 msec**. Estimate the TCP sending rate in bytes per second.

**Answer:**

- **Formula:** $TCP \text{ rate} \approx \frac{cwnd}{RTT}$.
- **Calculation:**
    - $cwnd = 64,000$ bytes.
    - $RTT = 0.1$ seconds.
    - $Rate = 64,000 / 0.1 = \mathbf{640,000 \text{ bytes/sec}}$.

---

### **6. Bottleneck Fairness**

**Question:** **Five** TCP sessions share a bottleneck link with a total bandwidth ($R$) of **1 Gbps**. According to the TCP fairness goal, what is the fair average rate for each session?

**Answer:**

- **Formula:** $Fair \text{ rate} = R / K$.
- **Calculation:**
    - $R = 1,000$ Mbps.
    - $K = 5$.
    - $Fair \text{ rate} = 1,000 / 5 = \mathbf{200 \text{ Mbps}}$.
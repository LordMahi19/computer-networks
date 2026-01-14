Based on the provided excerpts from "slide 4.pdf," the mathematical formulas and quantitative expressions mentioned are as follows:

- **Switching Rate Capacity:** In the context of generic router architecture, the ideal switching rate is expressed as **$NR$**, where $N$ represents the number of inputs and $R$ is the line rate.
- **Buffer Size Recommendations:**
    - A traditional "rule of thumb" (RFC 3439) for average buffering is calculated as **$\text{RTT} \cdot C$**, where RTT is the round-trip time (often cited as 250 msec) and $C$ is the link capacity. For example, a 10 Gbps link would require a 2.5 Gbit buffer.
    - A more recent recommendation for buffering when there are $N$ flows is calculated using the formula **$\frac{\text{RTT} \cdot C}{\sqrt{N}}$**.
- **Weighted Fair Queuing (WFQ):** Each traffic class $i$ is assigned a weight $w_i$ and receives a weighted amount of service in each cycle according to the formula **$\frac{w_i}{\sum w_j}$**. This formula is used to ensure a minimum bandwidth guarantee per traffic class.
- **IP Header Overhead:** The total overhead for a packet using both TCP and IP is expressed as **$20 \text{ bytes TCP} + 20 \text{ bytes IP} = 40 \text{ bytes}$**.
- **Binary to Decimal Conversion:** To illustrate dotted-decimal IP notation, the sources provide a calculation for converting an 8-bit binary string to its decimal equivalent: **$2^7 + 2^6 + 2^5 + 2^4 + 2^3 + 2^2 + 2^1 + 2^0 = 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255$**.
- **Address Formats:** CIDR (Classless InterDomain Routing) uses the address format **$a.b.c.d/x$**, where $x$ represents the number of bits in the subnet portion of the address.




Based on the information provided in the sources, specifically the **Network Layer: Data Plane** section (Slide 4), here are several practice questions and answers to help you apply the mentioned mathematical formulas.

### **1. Switching Rate Capacity**

**Question:** A generic router architecture has **$N = 8$** input ports, each operating at a line rate of **$R = 40$ Gbps**. What is the ideal switching rate required for this router to ensure it is not a bottleneck?

- **Formula:** Ideal Switching Rate = **$NR$**.
- **Answer:** $8 \times 40 \text{ Gbps} = \mathbf{320 \text{ Gbps}}$.
- **Insight:** The switching rate is often measured as a multiple of the input/output line rate to ensure packets move efficiently from inputs to outputs.

---

### **2. Buffer Size: Traditional Rule of Thumb**

**Question:** According to **RFC 3439**, how much average buffering is recommended for a link with a capacity (**$C$**) of **100 Gbps** and a "typical" Round-Trip Time (**RTT**) of **250 msec**?

- **Formula:** Buffering = **$\text{RTT} \cdot C$**.
- **Answer:** $0.250 \text{ seconds} \times 100 \text{ Gbps} = \mathbf{25 \text{ Gbits}}$.
- **Insight:** While this rule of thumb is common, the sources note that too much buffering can increase delays and harm the performance of real-time applications.

---

### **3. Buffer Size: Recent Recommendation for $N$ Flows**

**Question:** If the same **100 Gbps** link from the previous question supports **$N = 400$** concurrent flows, what is the more recent recommendation for the buffer size?

- **Formula:** Buffering = **$\frac{\text{RTT} \cdot C}{\sqrt{N}}$**.
- **Answer:** $\frac{250 \text{ msec} \cdot 100 \text{ Gbps}}{\sqrt{400}} = \frac{25 \text{ Gbits}}{20} = \mathbf{1.25 \text{ Gbits}}$.

---

### **4. Weighted Fair Queuing (WFQ)**

**Question:** A router uses WFQ to manage three traffic classes with the following weights: **$w_1 = 2$**, **$w_2 = 3$**, and **$w_3 = 5$**. If the total link capacity is **10 Gbps**, what is the minimum bandwidth guarantee for **Class 2**?

- **Formula:** Service Fraction = **$\frac{w_i}{\sum w_j}$**.
- **Answer:**
    1. Calculate total weight: $2 + 3 + 5 = 10$.
    2. Calculate Class 2 fraction: $\frac{3}{10} = 0.3$.
    3. Calculate bandwidth: $0.3 \times 10 \text{ Gbps} = \mathbf{3 \text{ Gbps}}$.

---

### **5. IP Header Overhead**

**Question:** A host is sending an application-layer message. If the packet uses the standard **TCP** and **IPv4** headers, what is the total overhead added to the application data?

- **Formula:** Total Overhead = **$20 \text{ bytes (TCP)} + 20 \text{ bytes (IP)}$**.
- **Answer:** **$40 \text{ bytes}$** (plus any additional application-layer overhead).

---

### **6. Binary to Decimal Conversion**

**Question:** Convert the 8-bit binary segment **`11001000`** to its dotted-decimal equivalent.

- **Formula:** **$\sum (\text{bit} \cdot 2^i)$** where $i$ is the bit position (0-7).
- **Answer:** $(1 \cdot 2^7) + (1 \cdot 2^6) + (0 \cdot 2^5) + (0 \cdot 2^4) + (1 \cdot 2^3) + (0 \cdot 2^2) + (0 \cdot 2^1) + (0 \cdot 2^0)$ $= 128 + 64 + 0 + 0 + 8 + 0 + 0 + 0 = \mathbf{200}$.
- **Insight:** This conversion is used to translate 32-bit IP addresses into the human-readable **dotted-decimal notation**.

---

### **7. CIDR Address Format**

**Question:** Given the CIDR address **`200.23.16.0/23`**, how many bits are used for the **subnet part** and how many are left for the **host part**?

- **Formula:** Format **$a.b.c.d/x$**, where **$x$** is the number of bits in the subnet portion.
- **Answer:**
    - **Subnet part:** **23 bits** (as indicated by the $/23$).
    - **Host part:** $32 - 23 = \mathbf{9 \text{ bits}}$.
- **Insight:** In CIDR, the subnet portion can be of arbitrary length, which allows for more efficient **hierarchical addressing** and **route aggregation**.
Based on the sources, the following mathematical formulas and expressions are mentioned in the material associated with **slide 5.pdf**:

### **Cyclic Redundancy Check (CRC)**

The sources provide several formulas used to calculate and verify the bit pattern for Error Detection and Correction (EDC) using CRC:

- The relationship for the bit pattern is expressed as **`<D,R> = D 2r XOR R`**.
- To ensure the data and CRC bits are exactly divisible by the generator (G) using modulo 2 arithmetic, the goal is to satisfy **`D.2r XOR R = nG`**.
- An equivalent expression provided is **`D.2r = nG XOR R`**.
- To find the remainder (R), the formula used is **`R = remainder [ D.2r / G ]`**.

### **Multiple Access Protocols Efficiency**

The sources contain formulas to calculate the performance and efficiency of different channel-sharing protocols:

**Slotted ALOHA**

- The probability that a given node has a successful transmission in a specific slot is **`p(1-p)N-1`**.
- The probability that any of the $N$ nodes has a success in a slot is **`Np(1-p)N-1`**.
- The **maximum efficiency** of Slotted ALOHA, determined by taking the limit as the number of nodes ($N$) goes to infinity, is calculated as **`1/e = .37`**.

**CSMA/CD**

- The efficiency of Carrier Sense Multiple Access with Collision Detection (CSMA/CD) is given by the formula **`efficiency = 1 / (1 + 5 * tprop / ttrans)`**, where $t_{prop}$ is the maximum propagation delay and $t_{trans}$ is the time to transmit a maximum-size frame.

### **Taking Turns Protocols**

- While not a complex formula, the sources specify that when **$M$ nodes** want to transmit using an ideal multiple access protocol, each should be able to send at an average rate of **`R/M`**, where $R$ is the total channel rate.




To help you practice the mathematical formulas found in the sources, here are several practice questions and their step-by-step answers based on the material in **slide 5.pdf**.

### **Practice Set 1: Cyclic Redundancy Check (CRC)**

**Question:** A sender wants to transmit the data bit pattern **$D = 101110$**. The generator used is **$G = 1001$**, and the degree of $G$ is **$r = 3$**. Calculate the remainder **$R$** and the final bit pattern to be transmitted.

**Answer:**

1. **Append $r$ zeros to $D$:** Since $r=3$, the modified data becomes $D \cdot 2^r = 101110000$.
2. **Perform Modulo 2 Division:** Divide $101110000$ by $G (1001)$ using XOR operations.
    - $101110000$ XOR $100100000 \rightarrow 001010000$
    - Continue dividing the remainder until the remaining bits are fewer than $G$.
3. **Find $R$:** Following the division process described in the sources, the remainder **$R$** is the result of $R = \text{remainder}[D \cdot 2^r / G]$.
4. **Final Pattern:** The transmitted bit pattern is **$<D, R>$**, which is mathematically expressed as **$D \cdot 2^r \text{ XOR } R$**.

---

### **Practice Set 2: Slotted ALOHA Efficiency**

**Question:** In a network using **Slotted ALOHA**, there are **$N = 15$** nodes. Each node transmits in a given slot with a probability of **$p = 0.05$**.

1. What is the probability that a _specific_ node has a successful transmission in a slot?
2. What is the probability that _any_ of the nodes has a success?

**Answer:**

1. **Specific Node Success:** The formula is **$p(1-p)^{N-1}$**.
    - Calculation: $0.05 \cdot (1 - 0.05)^{15-1} = 0.05 \cdot (0.95)^{14}$.
2. **Any Node Success:** The formula is **$Np(1-p)^{N-1}$**.
    - Calculation: $15 \cdot 0.05 \cdot (0.95)^{14}$.

**Bonus Question:** What is the maximum possible efficiency of this channel if the number of nodes becomes very large? **Answer:** The limit as $N$ goes to infinity is **$1/e$**, which is approximately **$0.37$** or **$37%$**.

---

### **Practice Set 3: CSMA/CD Efficiency**

**Question:** A Local Area Network (LAN) uses **CSMA/CD**. The maximum propagation delay ($t_{prop}$) between any two nodes is **$10$ microseconds**, and the time to transmit a maximum-size frame ($t_{trans}$) is **$80$ microseconds**. Calculate the efficiency of this channel.

**Answer:**

1. **Identify the Formula:** The sources define efficiency as **$1 / (1 + 5 \cdot t_{prop} / t_{trans})$**.
2. **Plug in the Values:**
    - $\text{Efficiency} = 1 / (1 + 5 \cdot (10/80))$
    - $\text{Efficiency} = 1 / (1 + 5 \cdot 0.125)$
    - $\text{Efficiency} = 1 / (1 + 0.625) = 1 / 1.625 \approx \mathbf{0.615}$ **(or 61.5%)**.

---

### **Practice Set 4: Ideal Multiple Access**

**Question:** A broadcast channel has a total transmission rate **$R = 100 \text{ Mbps}$**. If there are **$M = 10$** nodes that all want to transmit data simultaneously, what is the average rate each node should be able to send at, according to the desiderata of an **ideal multiple access protocol**?

**Answer:**

1. **Identify the Requirement:** According to the sources, an ideal protocol ensures that when $M$ nodes want to transmit, each can send at an average rate of **$R/M$**.
2. **Calculation:**
    - $100 \text{ Mbps} / 10 \text{ nodes} = \mathbf{10 \text{ Mbps}}$ **per node**.

---

### **Practice Set 5: CSMA/CD Binary Backoff**

**Question:** A node on an Ethernet network has just experienced its **$m = 3\text{rd}$ collision**. According to the **binary (exponential) backoff algorithm**, from what set of integers will the node choose its random value **$K$**?

**Answer:**

1. **Identify the Rule:** After the $m$-th collision, a node chooses $K$ at random from the set **${0, 1, 2, \dots, 2^m - 1}$**.
2. **Application:** For $m=3$:
    - $2^3 - 1 = 8 - 1 = 7$.
    - The set is **${0, 1, 2, 3, 4, 5, 6, 7}$**.
3. **Wait Time:** The node will then wait **$K \cdot 512$** bit times before attempting to retransmit.
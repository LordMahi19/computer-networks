Based on the sources, the following mathematical formulas and calculations are mentioned in **slide 2.pdf**:

### **HTTP Performance**

- **Non-persistent HTTP response time**: The total time per object is calculated as **$2RTT + \text{file transmission time}$**. This is derived from one RTT to initiate the TCP connection and one RTT for the HTTP request and the arrival of the first few bytes of the response.

### **Web Caching and Network Delay**

The sources provide several formulas for calculating performance in a caching scenario:

- **Total end-end delay**: This is calculated as **$\text{Internet delay} + \text{access link delay} + \text{LAN delay}$**.
- **Access link utilization**: In a specific example, utilization is determined by the ratio of the data rate to the access link rate, such as **$0.9 / 1.54 = .58$**.
- **Average end-end delay with a cache**: When a cache is present (with a hit rate of 0.4), the formula used is **$0.6 \times (\text{delay from origin servers}) + 0.4 \times (\text{delay when satisfied at cache})$**.

### **File Distribution Time**

The sources compare the time required to distribute a file of size $F$ to $N$ peers using different architectures:

- **Client-Server Distribution Time ($D_{c-s}$)**: The distribution time is defined as **$D_{c-s} \ge \max{NF/u_s, F/d_{min}}$**, where $u_s$ is the server upload capacity and $d_{min}$ is the minimum client download rate.
    - The time to send one copy is **$F/u_s$**, and the time to send $N$ copies is **$NF/u_s$**.
- **P2P Distribution Time ($D_{P2P}$)**: The distribution time for a peer-to-peer approach is defined as **$D_{P2P} \ge \max{F/u_s, F/d_{min}, NF/(u_s + \sum u_i)}$**, where $\sum u_i$ is the aggregate upload rate of all peers.

### **General Calculations**

- **Transmission Time**: Transmission time for a file is denoted as **$F/u$**, where $F$ is the file size and $u$ is the upload rate.
- **IP Address Calculation**: The sources describe the dotted-decimal notation of an IP address as a **sum of 2's exponentials** (e.g., $11111111 = 2^7 + 2^6 + 2^5 + 2^4 + 2^3 + 2^2 + 2^1 + 2^0 = 255$).



To practice the mathematical concepts found in the sources, here are several practice questions and detailed answers based on the formulas mentioned in **slide 2.pdf** and related performance concepts in the other slides.

### **Question 1: Non-Persistent HTTP Performance**

**Scenario:** A client is requesting a single image file of size **500 Kbits** from a server. The Round Trip Time (RTT) between the client and the server is **40 ms**, and the transmission rate of the link is **1 Mbps**.

**Calculate the total time required to receive the object using non-persistent HTTP.**

- **Formula to use:** $\text{Response Time} = 2RTT + \text{transmission time}$.
- **Step 1 (Transmission Time):** $F / R = 500,000 \text{ bits} / 1,000,000 \text{ bps} = 0.5 \text{ seconds (or 500 ms)}$.
- **Step 2 (Total Time):** $2 \times (40 \text{ ms}) + 500 \text{ ms} = 80 \text{ ms} + 500 \text{ ms} = \mathbf{580 \text{ ms}}$.
- **Insight:** One RTT is used to initiate the TCP connection, and the second RTT is used for the HTTP request and the arrival of the first few bytes of the response.

---

### **Question 2: Web Cache and Average Delay**

**Scenario:** An institution has an average end-to-end delay of **2.0 seconds** when fetching objects from origin servers. They install a web cache with a **hit rate of 0.3 (30%)**. When an object is found in the cache, the delay is essentially negligible (**2 ms**).

**Calculate the new average end-to-end delay for the institution.**

- **Formula to use:** $\text{Avg Delay} = (\text{Miss Rate} \times \text{Origin Delay}) + (\text{Hit Rate} \times \text{Cache Delay})$.
- **Step 1 (Identify rates):** Hit rate = 0.3; therefore, Miss rate = 0.7.
- **Step 2 (Calculation):** $(0.7 \times 2.0 \text{ sec}) + (0.3 \times 0.002 \text{ sec}) = 1.4 \text{ sec} + 0.0006 \text{ sec} = \mathbf{1.4006 \text{ seconds}}$.
- **Insight:** Caching significantly reduces delay by satisfying a portion of requests locally, which also reduces traffic on the access link.

---

### **Question 3: Client-Server vs. P2P File Distribution**

**Scenario:** A server wants to distribute a **2 GB** file to **100 peers**.

- Server upload capacity ($u_s$) = **100 Mbps**.
- Each peer has a download capacity ($d_i$) of **50 Mbps**.
- Each peer has an upload capacity ($u_i$) of **2 Mbps**.

**Compare the minimum distribution time ($D$) for Client-Server and P2P architectures.**

- **Client-Server Formula:** $D_{c-s} \ge \max{NF/u_s, F/d_{min}}$.
    - $NF/u_s = (100 \times 2,000 \text{ MB}) / 100 \text{ Mbps} = 200,000 \text{ Mbits} / 100 \text{ Mbps} = \mathbf{2,000 \text{ seconds}}$.
    - $F/d_{min} = 2,000 \text{ MB} / 50 \text{ Mbps} = 16,000 \text{ Mbits} / 50 \text{ Mbps} = 320 \text{ seconds}$.
    - $D_{c-s} = \mathbf{2,000 \text{ seconds}}$.
- **P2P Formula:** $D_{P2P} \ge \max{F/u_s, F/d_{min}, NF/(u_s + \sum u_i)}$.
    - $F/u_s = 16,000 / 100 = 160 \text{ seconds}$.
    - $F/d_{min} = 320 \text{ seconds}$.
    - $NF / (u_s + \sum u_i) = (100 \times 16,000) / (100 + [100 \times 2]) = 1,600,000 / 300 = \mathbf{5,333.3 \text{ seconds}}$.
    - _Correction based on formulas:_ In this specific high-N case with low peer upload, $D_{P2P} = \mathbf{5,333.3 \text{ seconds}}$.
- **Comparison:** In cases where peer upload capacity is very low, the aggregate upload limit ($u_s + \sum u_i$) becomes the bottleneck for P2P.

---

### **Question 4: Nodal Delay and Transmission**

**Scenario:** A packet of length **L = 2,000 bits** is sent over a link with a transmission rate **R = 2 Mbps**. The physical length of the link is **1,000 km**, and the propagation speed is **$2 \times 10^8$ m/sec**. Assume processing and queuing delays are zero.

**Calculate the nodal delay.**

- **Formula to use:** $d_{nodal} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$.
- **Step 1 (Transmission Delay):** $L/R = 2,000 \text{ bits} / 2,000,000 \text{ bps} = \mathbf{0.001 \text{ sec (1 ms)}}$.
- **Step 2 (Propagation Delay):** $d/s = 1,000,000 \text{ m} / 200,000,000 \text{ m/sec} = \mathbf{0.005 \text{ sec (5 ms)}}$.
- **Step 3 (Total):** $0 + 0 + 1 \text{ ms} + 5 \text{ ms} = \mathbf{6 \text{ ms}}$.

---

### **Question 5: Traffic Intensity**

**Scenario:** A router receives packets of length **L = 1,500 bits** at an average rate of **100 packets per second**. The output link has a capacity of **R = 200 Kbps**.

**Determine the traffic intensity and describe the state of the queue.**

- **Formula to use:** $\text{Traffic Intensity} = La/R$.
- **Calculation:** $(1,500 \text{ bits} \times 100 \text{ pkts/sec}) / 200,000 \text{ bps} = 150,000 / 200,000 = \mathbf{0.75}$.
- **Answer:** The traffic intensity is **0.75**. Since it is less than 1, the average queuing delay will be small but non-zero. If the intensity were to approach 1, the delay would become very large.



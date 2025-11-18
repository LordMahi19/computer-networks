# TCP Congestion Control

TCP congestion control is a critical algorithm that prevents the Internet from collapsing under heavy load. It allows a TCP sender to regulate its sending rate based on perceived network congestion.

### How TCP Perceives Congestion
TCP uses **packet loss** as the primary signal of congestion. It detects loss in two ways:
1.  **Timeout:** A retransmission timer expires before an ACK is received. This is treated as a sign of severe congestion.
2.  **Triple Duplicate ACKs:** The sender receives three ACKs for the same sequence number. This is treated as a sign of milder congestion, as packets are still flowing.

### The Congestion Window (cwnd)
TCP controls its sending rate by limiting the number of unacknowledged bytes it can have in the network. This limit is called the **congestion window (cwnd)**.
- **Sending Rate:** The effective sending rate of a TCP sender is approximately **`cwnd / RTT`**.
- **Goal:** The goal of the congestion control algorithm is to adjust `cwnd` to match the available capacity of the network path.

### The TCP Congestion Control Algorithm
The algorithm is a combination of three main phases, often summarized as **AIMD (Additive Increase, Multiplicative Decrease)**.

1.  **Slow Start (Exponential Growth):**
    - **When it happens:** At the beginning of a connection, or after a timeout-detected loss.
    - **Goal:** To quickly ramp up the sending rate to find the available bandwidth.
    - **Mechanism:**
        - `cwnd` is initialized to 1 MSS (Maximum Segment Size).
        - For every ACK received, `cwnd` is incremented by 1 MSS.
        - This results in the `cwnd` **doubling approximately every RTT**.
    - **When it ends:** The exponential growth stops when `cwnd` reaches a threshold value called `ssthresh` (slow start threshold), or when a loss event occurs.

2.  **Congestion Avoidance (Linear Growth):**
    - **When it happens:** When `cwnd` is greater than `ssthresh`.
    - **Goal:** To probe for new bandwidth more cautiously.
    - **Mechanism:** `cwnd` is increased by just 1 MSS per RTT. This is the **Additive Increase** part of AIMD.

3.  **Congestion Detection (Rate Reduction):**
    - **When it happens:** When a loss event is detected.
    - **Goal:** To quickly reduce the sending rate in response to congestion.
    - **Mechanism:**
        - **Case 1: Timeout Occurs (Severe Congestion)**
            - `ssthresh` is set to `cwnd / 2`.
            - `cwnd` is reset to **1 MSS**.
            - The sender re-enters the **Slow Start** phase.
        - **Case 2: Triple Duplicate ACK Occurs (Milder Congestion)**
            - This triggers **Fast Retransmit** and **Fast Recovery**.
            - `ssthresh` is set to `cwnd / 2`.
            - `cwnd` is also set to `cwnd / 2`.
            - The sender immediately enters the **Congestion Avoidance** phase.
            - This is the **Multiplicative Decrease** part of AIMD.

### Fairness
- TCP's AIMD algorithm promotes fairness. If multiple TCP connections are sharing a bottleneck link, they will tend to converge to an equal share of the link's bandwidth.
- However, applications using **UDP** do not have built-in congestion control. A UDP source can blast data at a constant rate, potentially starving TCP connections of bandwidth.

### Modern TCP Variants
- **TCP CUBIC:** The default in Linux and other systems. It uses a cubic function to govern window growth, allowing it to be more aggressive in high-bandwidth, long-delay networks.
- **BBR (Bottleneck Bandwidth and Round-trip propagation time):** A newer algorithm developed by Google. Instead of using loss as the signal for congestion, BBR attempts to measure the bottleneck bandwidth and RTT to keep the "pipe" just full without creating large queues.

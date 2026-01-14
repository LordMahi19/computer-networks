# 1. How is UDP socket fully identified? What about TCP socket What is the difference between the full identification of both sockets? 

A **UDP socket** is identified by a ==**two-tuple**== (destination IP address and destination port number). Consequently, all incoming UDP segments with the same destination port are directed to the **same socket**, regardless of their source.

A **TCP socket** is identified by ==a **four-tuple**== (source IP, source port, destination IP, and destination port). This allows a server to support multiple simultaneous connections to the same port (like port 80) by directing segments with different source information to **different sockets**.

The primary **difference** is that TCP uses the source IP and port to distinguish between connections, whereas UDP relies solely on the destination information for demultiplexing.

# 2. Describe why an application developer might choose to run an application over UDP rather than TCP.

An application developer might choose **User Datagram Protocol (UDP)** over TCP for the following reasons:

- **No Connection Delay:** UDP is **connectionless**, meaning it requires no initial handshaking, which avoids the ==Round-Trip Time (RTT)== delay needed to establish a connection,.
- **Finer Control Over Timing:** Because UDP lacks **congestion control**, rate-sensitive applications can "blast away" data at a constant speed without being throttled by the protocol during network overloads,.
- **Reduced Overhead:** UDP is simpler than TCP, featuring a **small header size** and requiring no maintained connection state at the sender or receiver.
- **Suitability for Loss-Tolerant Apps:** It is ideal for applications that can tolerate some data loss but require low delay, such as **streaming multimedia, interactive games, and DNS**,.
- **Custom Reliability:** Developers can choose to implement their own error and congestion control specifically at the **application layer** if the standard TCP mechanisms are unsuitable,.


# 3. Why is it that voice and video traffic is often sent over TCP rather than UDP in today’s Internet? (Hint: The answer we are looking for has nothing to do with TCP’s congestion-control mechanism.) 


Voice and video traffic frequently use TCP over UDP for the following reasons:
- **Works Better With Firewalls:** Many firewalls block UDP by default but allow normal web traffic (HTTP over TCP). TCP connections also play nicely with NAT in home and office routers, so they’re less likely to break.
    
- **Modern Streaming Uses It:** Today’s video streaming (like DASH) is built on top of HTTP, which itself uses TCP. So TCP becomes the natural choice.
    
- **Fits Existing Internet Infrastructure:** The Internet already has tons of caching systems and CDNs designed to deliver HTTP/TCP content efficiently, so using TCP lets streaming services take advantage of that.
    
- **Fast Enough Today:** Even though TCP doesn’t guarantee perfect timing, modern Internet speeds are usually high enough that “good enough” performance works fine for real‑time video.

# 4. Is it possible for an application to enjoy reliable data transfer even when the application runs over UDP? If so, how? 

# ✅ Yes — an app can get reliable data over UDP

UDP itself is **not reliable**, but an application can **add reliability on top of it**.

# How it works (super simple)

- **A==dd sequence numbers** → to detect missing or out‑of‑order packets==
- ==**Send ACKs** → receive==r confirms what arrived
- **Retransmit lost packets** → resend anything not acknowledged
- **Use timers** → detect when something is lost

This is exactly what protocols like **QUIC (used by HTTP/3)** do: they build a reliability layer _on top of_ UDP.

# 5. Suppose that a Web server runs in Host C on port 80. Suppose this Web server uses persistent connections, and is currently receiving requests from two different Hosts, A and B. Are all of the requests being sent through the same socket at Host C? If they are being passed through different sockets, do both of the sockets have port 80? Discuss and explain.

The requests are passed through **different sockets** at Host C. In TCP, each socket is uniquely identified by a **4-tuple** consisting of the source IP address, source port number, destination IP address, and destination port number.

Because Host A and Host B have different IP addresses, their 4-tuples are distinct, leading the server to create a **new socket** to communicate with each particular client. Both of these sockets on Host C use **port 80** as their local port, but they remain unique because the source IP address and/or source port number in the 4-tuple will differ for each connection. This allows the server to maintain multiple simultaneous persistent connections by demultiplexing incoming segments to the correct socket based on all four values.

# 6. Consider a stop-and-wait data-transfer protocol that provides error checking and retransmissions but uses only negative acknowledgments. Assume that negative acknowledgements are never corrupt. Would such protocol work over a channel with bit errors? What about over a lossy channel with bit errors?

## 🌱 **Simplified version of the question**

Imagine a very basic communication system where:

- You send **one packet at a time**.
- The receiver checks for errors.
- The receiver **only sends a message back if something is wrong** (a **negative acknowledgment**, or **NAK**).
- If the receiver gets the packet correctly, it stays silent.
- NAKs are guaranteed to never be corrupted.

Now the question asks:

### **1. If the communication channel sometimes flips bits (bit errors), will this system still work?**

Meaning:  
If a packet gets corrupted, the receiver will detect it and send a NAK.  
If the packet is fine, the receiver stays silent.

Is that enough to ensure reliable communication?

---

### **2. What if the channel not only flips bits but also sometimes _drops_ packets entirely (lossy channel)?**

Meaning:  
Sometimes the packet never reaches the receiver at all.  
Sometimes the NAK might also get lost.

Will the system still work in that case?

---

Here’s the answer in the simplest possible way — no jargon, just the core idea.

---

# ✅ **1. Channel with bit errors (but no packet loss)**

**Yes, the protocol works.**

Why?

- If a packet gets corrupted, the receiver notices and sends a **NAK**.
- Since NAKs are never corrupted, the sender always knows it must resend.
- If the packet arrives correctly, the receiver stays silent, and the sender knows everything is fine.

So:  
**Corruption is okay because the receiver can complain (NAK), and the sender can resend.**

---

# ❌ **2. Channel with packet loss + bit errors**

**No, the protocol does NOT work.**

Why?

- If a packet is **lost**, the receiver never sees it.
- Since the receiver never sees it, it cannot send a NAK.
- The sender hears _nothing_ — but silence could mean:
    - “Packet was received correctly”  
        **or**
    - “Packet never arrived at all”

The sender has no way to tell the difference.

So:  
**Packet loss breaks the system because silence becomes ambiguous.**

---

# ⭐ Final summary

|Situation|Does it work?|Why|
|---|---|---|
|**Bit errors only**|✅ Yes|Receiver sends NAK when needed|
|**Loss + bit errors**|❌ No|Lost packets produce no NAK, so sender gets stuck|

---
# 7. Consider a scenario in which Host A and Host B want to send messages to Host C. Hosts A and C are connected by a channel that can lose and corrupt (but not reorder) messages. Hosts B and C are connected by another channel (independent of the channel connecting A and C) with the same properties. The transport layer at Host C should alternate in delivering messages from A and B to the layer above (that is, it should first deliver the data from a packet from A, then the data from a packet from B, and so on). Design a stop-and-wait-like error-control protocol for reliably transferring packets from A and B to C, with alternating delivery at C as described above. Give FSM descriptions of A and C. (Hint: The FSM for B should be essentially the same as for A.) Also, give a description of the packet format(s) used.

Here’s the same question in simpler, more “plain language” terms:

You have three hosts: A, B, and C.

- **Host A → Host C:** Connected by a channel that can:
    
    - **Lose** packets
    - **Corrupt** packets
    - **But does NOT reorder** packets
- **Host B → Host C:** Connected by a **separate** channel with the **same properties**:
    
    - Can lose and corrupt packets, but not reorder them.

Host C is receiving messages from both A and B.

The requirement is:

- Host C must **deliver data to the application layer in strict alternation**:
    - First: deliver one correct packet’s data from **A**
    - Then: deliver one correct packet’s data from **B**
    - Then again from **A**, then from **B**, and so on…
- Even though packets can be lost or corrupted on the way, the protocol must ensure:
    - **Reliable transfer** (no missing or duplicated data)
    - **Correct alternation** (A, B, A, B, A, B, … in delivery order at C)

Your task is to:

1. **Design a stop-and-wait-like protocol** that works in this setting:
    
    - A and B each use a stop-and-wait style of sending to C.
    - C uses acknowledgments (ACKs) and maybe sequence numbers, etc.
    - The protocol must handle **loss** and **corruption** on each channel.
2. **Draw FSMs (finite state machines)** for:
    
    - **Host A** (sender side)
    - **Host C** (receiver side that also enforces alternation)
    - (Host B’s FSM is basically the same as A’s, so you don’t need to redraw it separately, just note that.)
3. **Describe the packet format(s)**:
    
    - What fields are in a packet from A/B to C? (e.g., data, sequence number, sender ID, checksum, etc.)
    - What fields are in ACKs from C back to A/B?

In other words:  
Design a reliable stop-and-wait protocol where **two senders** (A and B) send to **one receiver** (C), and C must **alternate** which sender’s data it delivers to the application, despite possible **loss and corruption** on the channels. Then express that design using FSMs for A and C, and specify what each packet looks like.

---
An **FSM** just means a **Finite State Machine** — but in this question, it’s much simpler than it sounds.

Think of an FSM as a **little flowchart** that shows:

- what **state** a sender or receiver is in
- what **event** happens (packet arrives, ACK arrives, timeout, etc.)
- what **action** it takes (send, resend, deliver, switch sender, etc.)
- which **state** it moves to next

That’s all.

---

# 🌟 In this problem, FSM = a simple diagram of behavior

### For **A** (and B), the FSM is basically:

- **State 1:** Waiting to send packet
- **State 2:** Waiting for ACK

And arrows show:

- If ACK arrives → switch state
- If timeout → resend
- If corrupted ACK → resend

---

### For **C**, the FSM is:

- **State 1:** Expecting a packet from **A**
- **State 2:** Expecting a packet from **B**

And arrows show:

- If correct packet from A → deliver → ACK → switch to B
- If correct packet from B → deliver → ACK → switch to A
- If wrong/corrupted packet → resend last ACK

---

# 🌟 Why FSMs are used

They help you **describe the protocol clearly** without writing long paragraphs.  
They show exactly:

- what the sender/receiver is waiting for
- what happens when something arrives
- how the system reacts to errors

---
# 🌟 The Problem in Plain English

- **A** and **B** both want to send messages to **C**.
- The connections can **lose** or **corrupt** packets.
- **C must deliver messages in this order:**  
    **A, then B, then A, then B, then A, then B… forever.**
- You must design a **stop‑and‑wait style** protocol (send one packet, wait for ACK).

---

# 🌟 The Core Idea

Think of C as a **traffic controller**:

- C expects **A’s packet first**.
- When it gets a correct packet from A, it delivers it and sends an ACK to A.
- Then C switches and waits for **B’s packet**.
- When it gets a correct packet from B, it delivers it and sends an ACK to B.
- Then it switches back to A.
- Repeat forever.

A and B each behave like a normal stop‑and‑wait sender:

- Send a packet with a sequence number.
- Wait for ACK.
- If no ACK or corrupted ACK → resend.

---

# 🌟 Packet Format (very simple)

Each packet from A or B to C contains:

|Field|Meaning|
|---|---|
|**senderID**|A or B|
|**seqNum**|0 or 1 (stop‑and‑wait)|
|**data**|the message|
|**checksum**|used to detect corruption|

ACK packets from C contain:

|Field|Meaning|
|---|---|
|**senderID**|A or B (who the ACK is for)|
|**seqNum**|the sequence number being acknowledged|
|**checksum**|for corruption detection|

---

# 🌟 FSM for A (and same for B)

A has **two states**:

### **State: WAIT_TO_SEND(seqNum)**

- Send packet with seqNum.
- Wait for ACK.

### **State: WAIT_FOR_ACK(seqNum)**

- If correct ACK arrives → flip seqNum (0→1 or 1→0), go back to WAIT_TO_SEND.
- If ACK lost or corrupted → resend packet.

This is just normal stop‑and‑wait.

---

# 🌟 FSM for C (the interesting part)

C has **two states**:

### **State: EXPECT_FROM_A**

- If a correct packet arrives **from A** with the expected seqNum:
    - Deliver data to application.
    - Send ACK to A.
    - Flip expected seqNum for A.
    - Switch to **EXPECT_FROM_B**.
- If packet is corrupted or wrong seqNum → resend last ACK to A.

### **State: EXPECT_FROM_B**

- Same logic but for B.
- After delivering B’s packet → switch back to **EXPECT_FROM_A**.

This enforces the **A → B → A → B** delivery order.

---

# 🌟 Why This Works

- A and B each reliably send using stop‑and‑wait.
- C only accepts the sender it is currently expecting.
- Even if packets are lost or corrupted, stop‑and‑wait handles retransmissions.
- Alternation is guaranteed because C switches states only after a correct packet.

---

If you want, I can also draw the FSM diagrams in text form or show an example timeline of packets being sent and acknowledged.


Based on the sources provided, the design for this reliable transfer protocol uses the principles of **rdt3.0**, a stop-and-wait protocol that accounts for packet corruption and loss using checksums, sequence numbers, acknowledgments (ACKs), and timers.

### **Packet Formats**

The protocol requires two types of packets: **Data Packets** sent by Hosts A and B, and **ACK Packets** sent by Host C.

1. **Data Packet (from A or B to C):**
    - **Sequence Number (1 bit):** Used to detect duplicate packets caused by lost ACKs or premature timeouts. It alternates between 0 and 1.
    - **Checksum:** Used by the receiver to detect bit errors in the packet.
    - **Payload (Data):** The actual message from the application layer.
2. **ACK Packet (from C to A or B):**
    - **ACK Number (1 bit):** Indicates the sequence number of the packet being acknowledged.
    - **Checksum:** Used by the sender to ensure the ACK itself was not corrupted.

---

### **FSM for Host A (and Host B)**

Host A and Host B follow the standard **rdt3.0 sender** finite state machine (FSM). They are independent of each other and only care about their own reliable delivery to C.

- **State 1: Wait for call 0 from above**
    - **Event:** `rdt_send(data)` is called.
    - **Action:** Create packet `sndpkt` with sequence 0 and a checksum; send it via `udt_send(sndpkt)`; start a countdown timer. Move to State 2.
- **State 2: Wait for ACK 0**
    - **Event:** `rdt_rcv(rcvpkt)` && (`corrupt(rcvpkt)` || `isACK(rcvpkt, 1)`).
    - **Action:** Ignore (wait for timeout or correct ACK).
    - **Event:** `timeout`.
    - **Action:** Resend `sndpkt` (seq 0) and restart the timer.
    - **Event:** `rdt_rcv(rcvpkt)` && `notcorrupt(rcvpkt)` && `isACK(rcvpkt, 0)`.
    - **Action:** Stop the timer. Move to State 3.
- **State 3: Wait for call 1 from above**
    - **Event:** `rdt_send(data)` is called.
    - **Action:** Create packet `sndpkt` with sequence 1 and a checksum; send it via `udt_send(sndpkt)`; start the timer. Move to State 4.
- **State 4: Wait for ACK 1**
    - **Event:** `rdt_rcv(rcvpkt)` && (`corrupt(rcvpkt)` || `isACK(rcvpkt, 0)`).
    - **Action:** Ignore.
    - **Event:** `timeout`.
    - **Action:** Resend `sndpkt` (seq 1) and restart the timer.
    - **Event:** `rdt_rcv(rcvpkt)` && `notcorrupt(rcvpkt)` && `isACK(rcvpkt, 1)`.
    - **Action:** Stop the timer. Move to State 1.

---

### **FSM for Host C**

Host C must alternate deliveries (A, then B, then A...). Because it must also handle potential duplicates from the sender that is currently "waiting its turn," its FSM must be more complex than a standard receiver. Host C identifies the source based on which channel the packet arrived from.

- **State 1: Wait for 0 from A**
    - **Event:** `rdt_rcv(pkt)` from A && `notcorrupt(pkt)` && `has_seq0(pkt)`.
    - **Action:** `extract(pkt, data)`, `deliver_data(data)`, send `ACK0` to Host A. Move to State 2.
    - **Event:** `rdt_rcv(pkt)` from A && (`corrupt(pkt)` || `has_seq1(pkt)`).
    - **Action:** Send `ACK1` to Host A (acknowledging the last correctly received packet from A to stop A from retransmitting).
    - **Event:** `rdt_rcv(pkt)` from B.
    - **Action:** Since it is A's turn, C cannot deliver B's data yet. It sends an ACK for the last sequence number it successfully delivered from B to prevent Host B from timing out and retransmitting indefinitely.
- **State 2: Wait for 0 from B**
    - **Event:** `rdt_rcv(pkt)` from B && `notcorrupt(pkt)` && `has_seq0(pkt)`.
    - **Action:** `extract(pkt, data)`, `deliver_data(data)`, send `ACK0` to Host B. Move to State 3.
    - **Event:** `rdt_rcv(pkt)` from B && (`corrupt(pkt)` || `has_seq1(pkt)`).
    - **Action:** Send `ACK1` to Host B (acknowledging the last correctly received packet from B).
    - **Event:** `rdt_rcv(pkt)` from A.
    - **Action:** Send `ACK0` to Host A (the last sequence delivered from A).
- **State 3: Wait for 1 from A**
    - **Event:** `rdt_rcv(pkt)` from A && `notcorrupt(pkt)` && `has_seq1(pkt)`.
    - **Action:** `extract(pkt, data)`, `deliver_data(data)`, send `ACK1` to Host A. Move to State 4.
    - **Event:** (Similar to State 1, but expecting Seq 1 from A).
- **State 4: Wait for 1 from B**
    - **Event:** `rdt_rcv(pkt)` from B && `notcorrupt(pkt)` && `has_seq1(pkt)`.
    - **Action:** `extract(pkt, data)`, `deliver_data(data)`, send `ACK1` to Host B. Move to State 1.
    - **Event:** (Similar to State 2, but expecting Seq 1 from B).

This FSM ensures that Host C only delivers data to the upper layer in the strictly alternating order of A then B, while using sequence-numbered ACKs to keep both senders synchronized and to handle packet loss or corruption on their independent channels.

# 8. We have said that an application may choose UDP for a transport protocol because UDP offers finer application control (than TCP) of what data is sent in a segment and when.

a.       Why does an application have more control of what data is sent in a segment?

b.      Why does an application have more control on when the segment is sent?

---
### **a. Control over what data goes into each segment**

UDP is a very simple protocol. It doesn’t try to manage congestion or control the flow of data like TCP does. When an application sends something using UDP, the transport layer just takes the message exactly as the application gives it, adds a small header, and sends it off as one segment.  
TCP, on the other hand, treats data as one long stream of bytes and may split or combine pieces however it wants. With UDP, the application decides exactly what each segment contains.

---
### **b. Control over when the segment is sent**

UDP also gives the application more control over _when_ data is sent. Since UDP has no congestion control or flow control, it never slows down or delays messages due to network conditions.  
It’s also connectionless—there’s no setup or handshake like TCP’s initial RTT delay. As soon as the application has data ready, it can send it immediately and as fast as it wants.

---
# question 9

```

Consider transferring an enormous file of L bytes from Host A to Host B. Assume an MSS of 536 bytes.

a. What is the maximum value of L such that TCP sequence numbers are not exhausted? Recall that the TCP sequence number field has 4 bytes.

b. For the L you obtain in (a), find how long it takes to transmit the file. Assume that a total of 66 bytes of transport, network, and data-link header are added to each segment before the resulting packet is sent out over a 155 Mbps link. Ignore flow control and congestion control so A can pump out the segments back to back and continuously. 
```

Absolutely — let’s make part (a) _as simple as possible_.

---

# ✅ **Part (a): Maximum L before TCP sequence numbers wrap**

### **Step 1 — How many sequence numbers exist?**

TCP uses a **32‑bit sequence number field**.

So total possible sequence numbers:

[ 2^{32} = 4,294,967,296 \text{ bytes} ]

This means TCP can count **4.29 billion bytes** before the sequence number wraps back to 0.

---

### **Step 2 — What does MSS have to do with this?**

MSS = **536 bytes of data per segment**.

But for part (a), we don’t care about segments —  
we care about **how many bytes can be sent before sequence numbers run out**.

Sequence numbers count **bytes**, not segments.

So the **maximum file size L** is simply:

[ L_{\max} = 2^{32} = 4,294,967,296 \text{ bytes} ]

---

### ⭐ **Final Answer (Part a)**

**The maximum L is 4,294,967,296 bytes (about 4.29 GB).**

Because TCP sequence numbers can only represent that many bytes before wrapping.

---

If you want, I can walk you through part (b) just as simply.
**Given Information:**

- ==**File size ($L$):** $2^{32}$ bytes.==
- ==**Maximum Segment Size ($MSS$):** 536 bytes.==
- ==**TCP sequence number field:** 4 bytes (32 bits).==
- ==**Total header size per segment:** 66 bytes (not mentioned in sources).==
- ==**Link transmission rate ($R$):** 155 Mbps ($155 \times 10^6$ bps).==

**a. Maximum value of $L$** The TCP sequence number is a 32-bit field in the segment header. These sequence numbers represent a count of the bytes in the data stream rather than a count of segments. A 32-bit field can represent $2^{32}$ unique values, which limits the total unique bytes in a stream before exhaustion. **$L_{max} = 2^{32}$ bytes = 4,294,967,296 bytes.**

**b. Transmission Time Calculation** The total number of segments required to transmit the file is determined by the file size divided by the MSS. $\text{Number of segments} = \lceil L / MSS \rceil = \lceil 4,294,967,296 / 536 \rceil = 8,012,999 \text{ segments}$. Each segment is encapsulated with transport, network, and data-link headers totalling 66 bytes. $\text{Total bytes per packet} = MSS + \text{Headers} = 536 \text{ bytes} + 66 \text{ bytes} = 602 \text{ bytes}$. The total number of bits to be transmitted is the product of the number of segments, the bytes per packet, and bits per byte. $\text{Total bits} = 8,012,999 \times 602 \times 8 = 38,590,603,184 \text{ bits}$. Transmission time is the total bits divided by the link transmission rate $R$. $\text{Time} = \frac{38,590,603,184 \text{ bits}}{155,000,000 \text{ bps}} = 248.97 \text{ seconds}$. **Total Transmission Time: 248.97 seconds.**


# 10. We know that TCP waits until it has received three duplicate ACKs before performing a fast retransmit (Section 3.5.4 page 275, in course book (8th edition)) . Why do you think the TCP designers chose not to perform a fast retransmit after the first duplicate ACK for a segment is received?

Here’s a cleaner, simpler version that keeps the meaning intact:

---

TCP waits for **three duplicate ACKs** (instead of just one) to tell the difference between **normal packet reordering** and **real packet loss**.

- **Why three?** One duplicate ACK usually just means packets arrived in the wrong order. But if three later packets arrive and each triggers a duplicate ACK, it’s a strong sign that the missing packet was actually lost.
- **Avoiding waste:** If TCP retransmitted after only one duplicate ACK, it would often resend packets that weren’t really lost. That wastes bandwidth and lowers throughput.
- **Faster recovery:** Waiting for three duplicate ACKs gives TCP enough confidence to resend quickly without waiting for a timeout, while still avoiding unnecessary retransmissions.

---

If you want, I can make it even shorter or more analogy-based.
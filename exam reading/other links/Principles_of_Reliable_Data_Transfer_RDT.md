# Principles of Reliable Data Transfer (RDT)

Reliable Data Transfer (RDT) is a fundamental challenge in computer networking. The goal is to provide a reliable service abstraction on top of an underlying channel that is inherently unreliable (it can lose, corrupt, or reorder packets). TCP is the Internet's primary RDT protocol.

We can understand RDT by progressively building a protocol, starting with simple assumptions and gradually making them more realistic.

### RDT 1.0: Perfectly Reliable Channel
- **Assumption:** The underlying channel never loses or corrupts packets.
- **Protocol:** The sender simply sends data, and the receiver simply receives it. No error handling is needed.

### RDT 2.0: Channel with Bit Errors
- **Assumption:** The channel can corrupt packets (flip bits), but it will not lose them.
- **Solution:** We need mechanisms for error detection and feedback.
    - **Error Detection:** Use a **checksum** to detect corrupted packets.
    - **Feedback:** The receiver sends feedback to the sender.
        - **ACK (Acknowledgement):** Sent by the receiver if a packet is received correctly.
        - **NAK (Negative Acknowledgement):** Sent by the receiver if a packet is received corrupted.
    - **Retransmission:** If the sender receives a NAK, it retransmits the last packet.
- **Problem:** What if the ACK or NAK is corrupted? The sender won't know what to do.

### RDT 2.1: Handling Garbled ACKs/NAKs
- **Solution:** Add a **sequence number** to each data packet.
    - The sender sends a packet with sequence number 0.
    - If the receiver gets a corrupted ACK/NAK, the sender simply retransmits the packet (with sequence number 0).
    - If the receiver gets a duplicate packet (e.g., it correctly received packet 0 but its ACK was lost, so the sender retransmitted), it can check the sequence number. It sees it has already received packet 0, so it discards the duplicate and sends another ACK.
    - A 1-bit sequence number (0 or 1) is sufficient for this stop-and-wait protocol.

### RDT 2.2: A NAK-Free Protocol
- **Idea:** Instead of sending NAKs, the receiver can just send an ACK for the *last correctly received packet*.
- **Protocol:**
    - Sender sends packet 0.
    - If receiver gets packet 0 correctly, it sends ACK 0.
    - If receiver gets a corrupted packet, it does nothing (or, in a more robust version, it sends an ACK for the previous packet, e.g., ACK 1 if it was expecting packet 0).
    - If the sender receives a duplicate ACK (e.g., it sent packet 0, but receives another ACK 1), it knows the receiver did not correctly receive packet 0, so it retransmits.

### RDT 3.0: Channel with Errors and Loss
- **Assumption:** The channel can now corrupt and lose packets (and ACKs).
- **Problem:** If a packet or its ACK is lost, the sender will wait forever.
- **Solution:** Introduce a **countdown timer**.
    - The sender starts a timer after sending a packet.
    - If the timer expires before an ACK is received, the sender assumes the packet (or its ACK) was lost and **retransmits** the packet.
    - This requires setting a reasonable **timeout interval**.

This combination of checksums, sequence numbers, ACKs, and timeouts forms the basis of reliable data transfer.

### Pipelining for Performance
RDT 3.0 is a **stop-and-wait** protocol. It sends one packet, waits for the ACK, then sends the next. This is very inefficient.

- **Pipelining:** Allows the sender to send multiple packets ("in-flight") without waiting for acknowledgements. This dramatically increases utilization.
- **Consequences:**
    - Requires a larger range of sequence numbers.
    - Requires buffering at both the sender and receiver.
- **Two main approaches to pipelined RDT:**
    1.  **Go-Back-N (GBN):** The sender can have up to $N$ unacknowledged packets in the pipeline. The receiver only sends cumulative ACKs. If a packet is lost, the sender retransmits that packet and *all subsequent packets*.
    2.  **Selective Repeat (SR):** The sender can have up to $N$ unacknowledged packets. The receiver acknowledges each packet individually. If a packet is lost, the sender only retransmits that specific packet. This is more efficient but more complex.

# Error Detection and Correction

Errors (flipped bits) can be introduced into frames as they are transmitted across a link, due to signal attenuation and electromagnetic noise. The link layer uses error detection and correction techniques to handle this.

### The Core Idea: Redundancy
All error detection and correction schemes work by adding redundant bits to the data. These extra bits are the **Error Detection and Correction (EDC)** bits.

### Parity Checking
This is one of the simplest forms of error detection.

- **Single Bit Parity:**
    - **How it works:** An extra bit (the parity bit) is added to a block of data. In **even parity**, the parity bit is chosen so that the total number of '1's in the block (including the parity bit) is even.
    - **Detection:** Can detect any **single bit error**. If one bit flips, the parity will be wrong.
    - **Limitation:** Cannot detect an even number of bit errors (e.g., two flipped bits).

- **Two-Dimensional Bit Parity:**
    - **How it works:** The data is arranged in a table. A parity bit is calculated for each row and each column.
    - **Detection and Correction:** This scheme can **detect and correct any single bit error**. If a single bit flips, it will cause a parity error in both its corresponding row and column. The intersection of the erroneous row and column pinpoints the flipped bit, which can then be corrected.
    - **Limitation:** Can detect, but not correct, some two-bit errors.

### Cyclic Redundancy Check (CRC)
CRC is a much more powerful and widely used error detection technique. It is used in many link-layer protocols, including Ethernet and WiFi.

- **Concept:** The sender and receiver agree on a specific $r+1$ bit pattern, called the **Generator ($G$)**.
- **Sender's Side:**
    1.  The sender takes the $d$-bit data payload ($D$) and appends $r$ zero bits to it.
    2.  It performs a binary division of this $d+r$ bit string by the generator $G$.
    3.  The $r$-bit remainder of this division is the **CRC code**.
    4.  The sender replaces the $r$ zero bits with this CRC code and transmits the resulting $d+r$ bit frame.
- **Receiver's Side:**
    1.  The receiver divides the received $d+r$ bit frame by the same generator $G$.
    2.  If the remainder is **non-zero**, the receiver knows an error has occurred and will drop the frame.
    3.  If the remainder is **zero**, the receiver assumes the frame is error-free and passes the data ($D$) to the network layer.

- **Power of CRC:**
    - Can detect all burst errors (a sequence of consecutive flipped bits) of length less than or equal to $r$.
    - Can detect any odd number of bit errors.
    - Can detect most larger burst errors with very high probability.

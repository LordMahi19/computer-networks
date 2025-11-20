# Multiplexing and Demultiplexing

Multiplexing and demultiplexing are fundamental functions of the transport layer that allow multiple network applications to run on a single host simultaneously.

### The Problem
A host may be running many different network applications at the same time (e.g., a web browser, an email client, a music streaming app). All the data for these applications arrives through a single network interface. The transport layer needs a way to ensure that data from the network is delivered to the correct application process.

### Demultiplexing (at the Receiver)
- **Job:** The job of demultiplexing is to take a transport-layer segment that has arrived from the network layer and deliver it to the correct socket (and therefore, the correct application process).
- **How it works:** The transport layer inspects the header fields in the segment to identify the receiving socket.
    - **Key Fields:** Source port number and destination port number.

### Multiplexing (at the Sender)
- **Job:** The job of multiplexing is to gather data chunks from different application sockets, encapsulate each with a header (containing source and destination port numbers), and pass the resulting segments to the network layer.

### How Demultiplexing Works

#### Connectionless Demultiplexing (UDP)
- A UDP socket is identified by a two-tuple: **(destination IP address, destination port number)**.
- When a UDP segment arrives at a host, the transport layer checks the **destination port number** in the segment and directs the segment to the socket associated with that port.
- **Important:** Multiple UDP clients can send data to the *same* server process. As long as the destination IP and port are the same, all segments will be directed to the same socket on the server, regardless of the source IP or port.

#### Connection-Oriented Demultiplexing (TCP)
- A TCP socket is identified by a four-tuple: **(source IP address, source port number, destination IP address, destination port number)**.
- When a TCP segment arrives at a host, the transport layer inspects *all four* of these fields to determine which socket should receive the segment.
- **Key Difference:** This allows a single server process to handle connections from many different clients simultaneously. Each client connection will have a unique four-tuple, and the server will create a new, dedicated socket for each connection. All segments for that specific connection will be directed to that dedicated socket.

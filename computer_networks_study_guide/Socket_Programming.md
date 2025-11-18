# Socket Programming

A **socket** is the programming interface that allows an application to send and receive data over a network. It acts as the "door" between the application process and the transport layer.

### Two Main Socket Types
The socket API provides two main types of transport service, corresponding to the two main transport protocols:

1.  **UDP Socket:**
    - Provides an **unreliable datagram** service.
    - **Connectionless:** There is no connection setup between the sender and receiver.
    - The sender must explicitly attach the destination IP address and port number to each packet.
    - Packets may arrive out of order or be lost.

2.  **TCP Socket:**
    - Provides a **reliable, in-order byte stream** service.
    - **Connection-oriented:** A three-way handshake must be performed to establish a connection before any data can be sent.
    - The connection is established between the client socket and a specific server socket.
    - Data is transferred as a continuous stream of bytes, as if through a "pipe". TCP handles all the details of breaking the stream into segments, ensuring reliability, and managing congestion.

### Socket Programming with UDP

- **Server Side:**
    1.  Create a UDP socket.
    2.  Bind the socket to a specific port number. The OS will use the host's IP address.
    3.  Wait to receive a datagram from a client. When a datagram arrives, the OS will also provide the client's IP address and port number.
    4.  Process the datagram and, if necessary, create a new datagram to send back to the client (using the client's IP and port).

- **Client Side:**
    1.  Create a UDP socket.
    2.  Create a datagram containing the message to be sent, and specify the server's IP address and port number.
    3.  Send the datagram.
    4.  Optionally, wait to receive a reply from the server.

### Socket Programming with TCP

- **Server Side:**
    1.  Create a **welcoming socket** (or listening socket).
    2.  Bind the welcoming socket to a specific port number.
    3.  Put the welcoming socket in listening mode, waiting for connection requests from clients.
    4.  When a client initiates a connection, the server accepts it. This creates a **new, dedicated connection socket** for communication with that specific client.
    5.  The server can then send and receive data with the client over this new connection socket. The welcoming socket remains open to listen for requests from other clients, allowing a single server to handle multiple clients simultaneously.

- **Client Side:**
    1.  Create a TCP socket.
    2.  Initiate a TCP connection to the server by specifying the server's IP address and port number. This completes the three-way handshake.
    3.  Once the connection is established, send and receive data to/from the server as a byte stream.
    4.  When finished, close the connection.

### Handling Timeouts
When writing network applications, especially those that implement reliable data transfer, it's crucial to handle cases where a packet or its acknowledgment is lost. This is done using timeouts.

- **`settimeout()`:** The socket API provides a `settimeout()` function.
- **`try-except`:** You can place the receive call (`recvfrom()`) inside a `try` block. If no packet is received within the timeout period, a timeout exception is raised, which can be caught by an `except` block. This allows the application to take corrective action, such as retransmitting the last packet.

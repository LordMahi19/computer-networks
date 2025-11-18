# Peer-to-Peer (P2P) Applications

In a Peer-to-Peer (P2P) architecture, there is no always-on, centralized server. Instead, end systems (called **peers**) communicate directly with each other to distribute content and services.

### P2P vs. Client-Server for File Distribution
Consider distributing a large file of size $F$ to $N$ peers.

- **Client-Server:**
    - The server must upload one copy of the file to each of the $N$ peers. Total upload required by server: $N \times F$.
    - The time to distribute the file is limited by the server's upload capacity and the download capacity of the slowest client.

- **P2P:**
    - The original server only needs to upload one copy of the file into the network.
    - As peers receive parts of the file, they can immediately start uploading those parts to other peers.
    - This creates **self-scalability**: the total upload capacity of the system grows as more peers join.
    - P2P distribution time is generally much lower than client-server time, especially for a large number of peers.

### BitTorrent
BitTorrent is a popular P2P protocol for file distribution.

- **Core Concepts:**
    - **Torrent:** The group of all peers participating in the distribution of a particular file.
    - **Tracker:** A central server that keeps track of the peers currently in the torrent. (Note: Modern "trackerless" systems exist that use a distributed hash table).
    - **Chunks:** The file is divided into many small, fixed-size pieces called chunks (e.g., 256 KB).

- **Chunk Exchange Mechanism:**
    1.  A new peer joins the torrent with no chunks. It registers with the tracker to get a list of other peers and starts requesting chunks from them.
    2.  **Rarest First:** To ensure all chunks remain available, peers tend to request the chunks that are the rarest among their neighbors.
    3.  **Tit-for-Tat:** To encourage peers to upload, BitTorrent uses a "tit-for-tat" incentive mechanism.
        - A peer provides chunks to the four other peers that are currently sending it chunks at the highest rate. These are its "unchoked" peers.
        - All other neighboring peers are "choked" (do not receive chunks).
        - The set of unchoked peers is re-evaluated every 10 seconds.
    4.  **Optimistic Unchoke:** Every 30 seconds, a peer randomly selects one additional neighbor and starts sending it chunks.
        - This gives newly joined peers a chance to get chunks and become a top-four uploader for others.
        - It helps discover new peers that might offer a better upload rate.

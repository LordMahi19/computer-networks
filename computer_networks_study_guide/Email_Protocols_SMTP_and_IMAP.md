# Email Protocols: SMTP and IMAP

Email is one of the oldest and most important Internet applications. It involves several components and protocols.

### Core Components
1.  **User Agents:** The software users interact with to read, compose, and send email (e.g., Microsoft Outlook, Apple Mail, or a web browser like Gmail).
2.  **Mail Servers:** The workhorses of the email system. Each recipient has a **mailbox** on a mail server. Servers also have a **message queue** to hold outgoing email messages that are waiting to be delivered.
3.  **Simple Mail Transfer Protocol (SMTP):** The protocol used for sending email messages from a client's mail server to a recipient's mail server.

### Simple Mail Transfer Protocol (SMTP)
- **Purpose:** SMTP is used to **push** email from a sender's mail server to the recipient's mail server. It is also used by the sender's user agent to send the email to their own mail server.
- **Transport:** It uses **TCP** on port **25** to ensure reliable data transfer.
- **Interaction Model:** SMTP uses a persistent connection and a command/response interaction model with human-readable ASCII commands.
    - **Handshaking:** The client and server introduce themselves (e.g., `HELO`).
    - **Transfer of Messages:** The client specifies the sender (`MAIL FROM`), the recipient (`RCPT TO`), and then provides the message content using the `DATA` command.
    - **Closure:** The client sends the `QUIT` command to terminate the session.
- **Message Format (RFC 2822):**
    - An email message consists of a **header** (with lines like `To:`, `From:`, `Subject:`) and a **body**.
    - The body must be in 7-bit ASCII. To send multimedia content, it must be encoded into ASCII (e.g., using the MIME standard).

### Mail Access Protocols
SMTP is a push protocol used to deliver mail *to* the recipient's server. A different protocol is needed for the recipient's user agent to retrieve the mail *from* their server.

1.  **IMAP (Internet Mail Access Protocol):**
    - **Functionality:** IMAP is a more powerful and complex protocol that allows users to manage their email directly on the server.
    - **Features:**
        - Messages are stored on the server, allowing a user to access their mail from multiple different devices.
        - Users can create and manage folders on the server.
        - Users can search for messages on the server.
        - It maintains user state across sessions (e.g., which messages have been read).

2.  **POP3 (Post Office Protocol - version 3):**
    - **Functionality:** POP3 is a much simpler protocol. It is typically used to download email from the server to the user's local machine.
    - **"Download and delete" mode:** By default, POP3 downloads the messages and then deletes them from the server. This makes it difficult to access mail from multiple devices.
    - **"Download and keep" mode:** An option exists to leave a copy on the server, but it lacks the advanced management features of IMAP.

3.  **HTTP:**
    - Many modern email services (like Gmail, Outlook.com) use HTTP as the mail access protocol. The user agent is a web browser, and it communicates with the mail server via HTTP to send and receive messages.

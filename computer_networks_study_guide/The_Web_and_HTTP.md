# The Web and HTTP

The World Wide Web is a classic client-server application. The protocol that powers it is the **HyperText Transfer Protocol (HTTP)**.

### HTTP Overview
- **Client/Server Model:** A **web browser** acts as the client, requesting objects from a **web server**.
- **TCP Transport:** HTTP uses TCP as its underlying transport protocol, typically on port 80.
- **Stateless Protocol:** The HTTP server maintains no information about past client requests. Each request is treated independently. This design simplifies server code but requires other mechanisms (like cookies) to maintain user sessions.

### HTTP Connections

1.  **Non-Persistent HTTP (HTTP/1.0):**
    - A separate TCP connection is established for each object requested.
    - **Process:**
        1.  Client opens a TCP connection to the server.
        2.  Client sends an HTTP request.
        3.  Server sends the HTTP response containing the object.
        4.  Server closes the TCP connection.
    - **Drawback:** Very inefficient. Each object requires **2 Round-Trip Times (RTTs)**: one to establish the TCP connection and one for the request/response. This creates high overhead.

2.  **Persistent HTTP (HTTP/1.1):**
    - The server leaves the TCP connection open after sending a response, allowing multiple objects to be sent over the same connection.
    - **Process:**
        1.  Client opens a TCP connection.
        2.  Client sends requests for multiple objects.
        3.  Server sends responses for those objects over the same open connection.
    - **Benefit:** Reduces the latency associated with connection setup, leading to much faster page load times.

### HTTP Message Format
HTTP messages are in human-readable ASCII text.

-   **Request Message:**
    -   **Request Line:** Contains the `method` (e.g., GET, POST), the `URL` of the object, and the `HTTP version`.
    -   **Header Lines:** Provide additional information (e.g., `Host`, `User-Agent`, `Accept-Language`).
    -   **Entity Body:** Used with methods like POST to send data to the server (e.g., from a web form).

-   **Response Message:**
    -   **Status Line:** Contains the `HTTP version`, a `status code` (e.g., 200), and a `status phrase` (e.g., OK).
    -   **Header Lines:** Provide information about the response (e.g., `Content-Type`, `Content-Length`, `Date`).
    -   **Entity Body:** Contains the requested object (e.g., the HTML file or image).

### Common HTTP Methods
- **GET:** Requests a representation of the specified resource.
- **POST:** Submits data to be processed to a specified resource (e.g., submitting a form).
- **HEAD:** Same as GET but without the response body. Used to check if an object has changed.
- **PUT:** Uploads a representation of the specified resource.

### Common HTTP Status Codes
- **200 OK:** The request succeeded.
- **301 Moved Permanently:** The requested resource has been permanently moved to a new URL.
- **400 Bad Request:** The server could not understand the request due to invalid syntax.
- **404 Not Found:** The server could not find the requested resource.
- **505 HTTP Version Not Supported:** The server does not support the HTTP protocol version used in the request.

### Cookies
Cookies are used to maintain state between a client and a stateless server.
1.  The server sends a `Set-cookie:` header in its response.
2.  The browser stores this cookie on the client's machine.
3.  The browser includes the `Cookie:` header in all subsequent requests to that server.
This allows the server to identify the user across multiple requests, enabling features like shopping carts, user login sessions, and recommendations.

### Web Caching (Proxy Servers)
A web cache is a network entity that satisfies HTTP requests on behalf of an origin web server.
- **How it works:** The cache stores copies of recently requested objects. When a browser requests an object, the request is first sent to the cache. If the cache has the object, it returns it directly without contacting the origin server.
- **Benefits:**
    - Reduces response time for clients.
    - Reduces traffic on an institution's access link to the Internet.
- **Conditional GET:** To ensure a cached object is up-to-date, a cache can use a `Conditional GET` request with an `If-modified-since:` header. If the object has not changed, the server responds with `304 Not Modified`, and the cache uses its local copy.

### HTTP/2 and HTTP/3
- **HTTP/2:** Aims to decrease delay by introducing features like server push and allowing multiple objects to be requested and sent in parallel over a single TCP connection, mitigating Head-of-Line (HOL) blocking.
- **HTTP/3:** A major evolution that runs over **QUIC (Quick UDP Internet Connections)** instead of TCP. This provides per-object error and congestion control, further reducing HOL blocking and improving performance, especially on lossy networks.

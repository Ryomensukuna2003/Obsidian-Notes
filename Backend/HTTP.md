## 1. HTTP Request & Response Basics

### HTTP Request

- **Components:**
    - **URL:** The resource location.
    - **Method:** Indicates the action to be performed. Common methods:
        - **GET** – Retrieve data.
        - **POST** – Create new data.
        - **PUT** – Update existing data.
        - **DELETE** – Remove data.
        - **PATCH** – Partially update data.
    - **Headers:** Provide metadata (e.g., Content-Type, Authorization).
    - **Body:** Contains the payload (for methods like POST, PUT, PATCH).

### HTTP Response

- **Components:**
    - **Status Code:** Indicates the outcome (e.g., 200 OK, 404 Not Found, 500 Internal Server Error).
    - **Headers:** Carry metadata such as caching policies and content type.
    - **Body:** Contains the returned data (HTML, JSON, etc.).

---

## 2. Evolution of HTTP Versions

### HTTP 1.0

- **Connection Behavior:**
    - **New TCP connection per request:** Each HTTP request creates its own TCP connection.
- **Performance:**
    - **Slow:** High overhead due to connection setup and teardown.
    - **Buffering:** Data is buffered until the complete response is received.
- **Use Cases:** Early web applications with low traffic.

### HTTP 1.1

- **Persistent Connections:**
    - **Keep-Alive Header:** Maintains a single TCP connection for multiple requests.
- **Performance Enhancements:**
    - **Low Latency:** Reduced overhead compared to 1.0.
    - **Chunked Transfer Encoding:** Enables streaming of responses.
    - **Pipelining:** Multiple requests can be sent without waiting for each response (though pipelining is disabled by default in many implementations due to head-of-line blocking issues).
- **Advantages:** Improved efficiency and better resource utilization over HTTP 1.0.

### HTTP 2.0

- **Key Features:**
    - **Compression:** Header compression (HPACK) minimizes overhead.
    - **Multiplexing:** Multiple concurrent requests/responses are handled over a single TCP connection, avoiding the “head-of-line” blocking seen in HTTP 1.x.
    - **Binary Framing:** Uses a binary protocol for more efficient parsing.
    - **Secure by Default:** Often deployed with TLS (though not mandated by the spec, most modern browsers enforce secure connections).
- **Benefits:** Reduced latency and improved throughput, making it well suited for modern web applications.

### HTTP 3.0

- **Building on HTTP 2.0:**
    - Retains many features of HTTP 2.0 such as compression and multiplexing.
- **Transport Layer Changes:**
    - **Replaces TCP with UDP + Congestion Control:** Uses the QUIC protocol, which is built on UDP.
    - **Advantages:**
        - Lower connection establishment latency.
        - Improved performance over lossy networks.
    - **Considerations:** UDP is inherently “lossy” and requires robust congestion control mechanisms.
- **Overall:** HTTP 3.0 is designed for even better performance on modern networks, especially in mobile and high-latency environments.

---

## 3. Summary & Key Points

- **HTTP Request/Response:**
    - Requests consist of a URL, method, headers, and an optional body.
    - Responses include a status code, headers, and a body.
- **HTTP 1.0 vs. HTTP 1.1:**
    - HTTP 1.0 creates a new connection for every request, while HTTP 1.1 uses persistent connections (Keep-Alive) and supports pipelining and chunked transfer.
- **HTTP 2.0 Enhancements:**
    - Introduces binary framing, multiplexing, and header compression to boost performance and reduce latency.
- **HTTP 3.0 Innovations:**
    - Moves from TCP to QUIC (UDP-based), further reducing latency and improving performance in challenging network conditions.

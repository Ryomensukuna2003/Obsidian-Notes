### **What Sets RPC Apart?**

RPC is designed to make **network calls look like local function calls** by abstracting:

- **Serialization & Deserialization**
- **Transport**

---

## **Problems with REST**

- No standardization; different clients can return data in **Java, Python, C++, Node.js, etc.**
- Requires different implementations for different ecosystems.

---

## **Why Use RPC?**

RPC standardizes communication between services **regardless of language, function invocation style, or ecosystem**. It ensures uniform interaction between services.

### **Stub & Interface Description Language**

- **Proto files (`.proto`)** define structured messages and service interfaces.
- **Stubs** are generated for each language to handle communication seamlessly.

---

## **Concerns with RPC**

- **Stubs need regeneration** when method signatures change.
- **Testing RPC is non-trivial** compared to REST.
- **Limited browser support**, making it less suited for frontend communication.
- **Shines in inter-server communication**, especially in microservices.

---

## **Why gRPC?**

- Optimized for **server-to-server** communication (e.g., Golang ↔ Java).
- **Not well-suited for web browsers** due to lack of native browser support.
- Without gRPC, we would need **different client libraries** for each server to work.

---

## **How gRPC Works**

- Uses **HTTP/2.0** for better efficiency.
- Handles multiple languages through **code generation** via **Protocol Buffers (Protobuf)**.
- Protobuf acts as a **contract** for structured communication.

### **Performance Advantage**

- **Protocol Buffers** serialize data into **binary**, improving network performance.
- Unlike REST, which transmits data as **JSON (text-based)**, gRPC sends **compact binary**, reducing bandwidth usage.

### **Why Not Just Use Compression (e.g., gzip) in REST?**

- gzip could be used, but it requires importing and configuring **gzip libraries** on every server, adding complexity.
- gRPC inherently uses **binary serialization**, making it more efficient **without extra dependencies**.






Serialization and Deserialization

it is converting data to common format so all languages can make sense out of it
**The CAP Theorem says you can only choose two out of three guarantees for a distributed system:**

- **Consistency:** All clients see the same data at the same time.
- **Availability:** Every request receives a response, even if some data is unavailable.
- **Partition Tolerance:** The system continues to operate even if parts of it become disconnected (network failures).

Think of it like this:

- **Consistency & Availability:** If you prioritize both, then a network partition will break consistency. Imagine one server has updated data but can't communicate with the others – clients might get inconsistent information.
- **Consistency & Partition Tolerance:** If you prioritize both, then availability suffers. To ensure consistency across partitions, you need to synchronize data, which takes time. This delay means some requests might not be answered immediately.

**In practice:**

Most distributed systems make trade-offs based on their needs:

- Some prioritize **consistency** (e.g., financial transactions).
- Others prioritize **availability** (e.g., social media).


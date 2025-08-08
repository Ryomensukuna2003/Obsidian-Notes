## 1. Scenario: Social Media App with Post & Like Features

- **Initial Setup & Challenges:**
    - **Single Instance:**
        - A social media app uses one EC2 MySQL instance to store user data, posts, likes, etc.
        - Workload is moderate (e.g., 100 writes per second, WPS) and is handled by vertical scaling (upgrading RAM, CPU).
    - **Scaling Issue:**
        - A viral post spikes traffic to 200 WPS — vertical scaling works as a temporary fix.
        - A subsequent viral event surges traffic to 10,000 WPS; hardware limits are reached, and a single instance becomes a bottleneck.
    - **Solution:**
        - Transition from vertical scaling to **horizontal scaling** by distributing the load across multiple instances using sharding.

---

## 2. Sharding vs. Partitioning: The Basics

### 2.1. Sharding

- **Definition:**  
    Distributing your database across multiple EC2 instances (shards) to spread the read/write load.
- **Example:**
    - Shard by user ID range:
        - **Shard 1:** User IDs 1–1,000,000
        - **Shard 2:** User IDs 1,000,001–2,000,000
        - _…and so on._
- **Purpose:**
    - Distribute high traffic among several servers.
    - Increase overall storage and improve availability (if one shard fails, others continue to serve data).

### 2.2. Partitioning

- **Definition:**  
    Splitting data within each shard to optimize organization and query performance.
- **Types:**
    - **Horizontal Partitioning:**
        - Splitting a table by rows.
        - _Example:_ Partition the `posts` table by date (e.g., recent posts vs. older posts) so that most queries (typically for recent content) hit a smaller, optimized partition.
    - **Vertical Partitioning:**
        - Splitting a table by columns.
        - _Example:_ Separate columns that are frequently accessed (like post summary, title, author, timestamp) from those that are large or less frequently accessed (such as full post text or media data).

---

## 3. Sharding and Partitioning in Action

- **Sharding Across Instances:**
    
    - Data is divided among multiple shards. For instance, if you have 100GB of data:
        - **Shard 1:** Stores data for Users A and C.
        - **Shard 2:** Stores data for Users B and E.
    - This reduces load on any single server and helps handle high traffic.
- **Partitioning Within Each Shard:**
    
    - **Horizontal Partitioning:**
        - Tables (e.g., `posts`) can be partitioned by date to quickly access recent posts.
    - **Vertical Partitioning:**
        - Split tables so that large, rarely queried data (e.g., images or long text) is separated from summary information, improving the speed of common queries.

---

## 4. Data Partitioning Methods

- **Horizontal Partitioning (Sharding at Table Level):**
    - Splits data by rows based on criteria such as date or user ID.
    - Benefits: Smaller dataset per partition, faster queries for common access patterns.
- **Vertical Partitioning:**
    - Splits data by columns, isolating heavy or infrequently accessed data.
    - Benefits: Improves performance on critical queries by reducing the amount of data read from disk.

_The choice between these methods depends on your application's load, access patterns, and specific use cases._

---

## 5. Advantages of Sharding

- **Scalability:**
    - Distributes high read/write loads across multiple servers.
- **Increased Storage Capacity:**
    - Adding new shards increases the total available storage.
- **High Availability:**
    - If one shard fails, other shards continue to operate, ensuring higher overall up-time.

---

## 6. Disadvantages and Challenges of Sharding

- **Operational Complexity:**
    - Requires managing multiple database instances and ensuring the sharding logic is correctly implemented.
- **Complex Cross-Shard Queries:**
    - Joining data across shards (e.g., fetching the most recent posts from all users) can be expensive and complex.
- **Data Consistency:**
    - Maintaining consistency across shards often requires additional architectural strategies, such as distributed transactions or eventual consistency models.

---

## 7. Diagram & Architecture Overview

### Diagram Overview

- **Shards:**  
    Imagine your 100GB database split into multiple shards. For instance:
    - **Shard 1:** Data for Users A and C.
    - **Shard 2:** Data for Users B and E.
- **Within Each Shard:**
    - Data is further partitioned (e.g., horizontal split by date for `posts`).

![[Pasted image 20250210222426.png|700]]

---

## 8. Summary

- **Vertical Scaling** provides a temporary fix for moderate load increases but reaches hardware limits.
- **Horizontal Scaling with Sharding** distributes the load across multiple servers, improving scalability and availability.
- **Partitioning** (horizontal and vertical) further organizes data within each shard to optimize query performance.
- **Trade-offs:**
    - While sharding handles high load and improves storage capacity, it introduces operational complexity and expensive cross-shard operations.
- **Design Considerations:**
    - Choose partitioning methods based on your data access patterns.
    - Be mindful of cross-shard query costs when designing application features.


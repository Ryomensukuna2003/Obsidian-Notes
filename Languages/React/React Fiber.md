### **1. Why Fiber Exists**

Before React 16, rendering was **synchronous** — once React started updating, it couldn’t stop until it was done.  
This caused **frame drops** for big updates.  
Fiber was introduced to make rendering **interruptible and prioritized**.

### **2. What is a Fiber?**

- A **Fiber** is a **JavaScript object** that represents a node in the Virtual DOM tree.
- It contains:
    - **Type** (`div`, Component, etc.)
    - **Props & state**
    - **Return pointer** (parent)
    - **Child pointer** (first child)
    - **Sibling pointer** (next element at the same level)
    - **Effect list** (what needs to be changed in DOM)
    - **Priority/expiration time** (so urgent work runs first)

Think of it like: **"A linked list + tree structure for the UI"**.


### **3. How Fiber Works in Rendering**

React rendering has **two phases** under Fiber:

#### **A. Render Phase (Reconciliation)**

- Builds the new Fiber tree from your components.
- Compares each Fiber with its old version (from previous render).
- Marks effects (changes) that need to be committed.
- **Can be paused, resumed, or aborted** — this is the key improvement.
- Uses a **work loop**:
    ```txt
    while (there is work and time left in this frame) {
        process next fiber
        if (deadline reached) pause
    }
    ```
- Uses **requestIdleCallback / scheduler** to yield to the browser if needed.

#### **B. Commit Phase**

- Takes the effect list and applies changes to the real DOM.
- This phase is **synchronous** (can’t be paused) because you don’t want partial UI changes visible.

### **4. Priority Levels**

Fiber assigns priorities:
- **Immediate** – For user interactions (typing, clicking)
- **User-blocking** – For animations, transitions
- **Normal** – For rendering non-urgent content
- **Low** – For prefetching, background tasks
- **Idle** – For non-visible updates

High-priority work can **interrupt** low-priority work before it’s done.

### **5. Why It Makes React Fast**

- **Splits work into chunks** → avoids blocking the main thread.
- **Prioritizes important updates** (like keeping typing responsive).
- **Skips unnecessary work** if newer updates come in before finishing old ones.
- Still benefits from **Virtual DOM diffing** for minimal DOM changes.
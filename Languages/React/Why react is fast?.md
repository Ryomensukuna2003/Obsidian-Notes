Whenever a local state variable is changed whole component re-renders but isn't this a costly operation?

**1. React’s speed comes from the Virtual DOM (VDOM) + diffing**

- The **real DOM** is slow to read/write because it involves layout, painting, and reflow in the browser.
- The **Virtual DOM** is just a lightweight JavaScript object tree representing the UI — creating and comparing JS objects is much faster than touching the real DOM.

**2. Diffing Algorithm**

- React keeps an **old VDOM** snapshot and creates a **new VDOM** on each render.
- It compares (diffs) these trees to figure out the minimal set of changes needed.
- Then it updates only those specific real DOM nodes.

**3. Reconciliation**

- The process of matching the old VDOM with the new one and applying updates to the real DOM.
- Keys in lists help React do this efficiently by matching elements across renders.

**4. [[React Fiber]]**

- Fiber is React’s internal architecture for rendering introduced in React 16.
- It breaks the work into small units (“fibers”) so it can pause and resume rendering, making UI more responsive, especially during large updates.
- Fiber also powers features like concurrent rendering and Suspense.


#### **React Update Flow (VDOM → Diffing → Reconciliation → DOM)**

1. **Trigger render**
    - A state or prop change happens.
    - React schedules an update (via Fiber).
        
2. **Render Phase (VDOM creation)**
    - React calls the component’s render function.
    - Creates a **new Virtual DOM tree** (lightweight JS objects).
    - Fiber breaks this work into units so it can pause/resume if needed.
        
3. **Diffing (VDOM comparison)**
    - React compares the **new VDOM** with the **previous VDOM snapshot**.
    - It uses:
        - **Type check**: If element type changes (e.g., `<div>` → `<span>`), replace the node.
        - **Key check**: For lists, keys match elements across renders.
        - **Shallow prop/state check**: If props/state change, update; else reuse.
            
4. **Reconciliation (deciding changes)**
    - React builds a **list of changes** (add, update, remove) needed in the real DOM.
    - This step does **not** touch the real DOM yet — it’s just planning.
        
5. **Commit Phase (real DOM update)**
    - React applies the minimal set of changes to the actual DOM.
    - Runs `componentDidMount` / `useEffect` after DOM updates.
        
6. **Browser paints**
    - The browser takes the updated DOM and renders it to the screen.




 ┌─────────────────────────┐
 │  State/Prop Changes     │
 └───────────┬─────────────┘
             ↓
 ┌─────────────────────────┐
 │ Fiber: Schedule Update  │
 └───────────┬─────────────┘
             ↓
 ┌─────────────────────────┐
 │ Render Phase            │
 │ - New VDOM created      │
 └───────────┬─────────────┘
             ↓
 ┌─────────────────────────┐
 │ Diffing Algorithm       │
 │ - Compare old vs new    │
 │ - Track changes         │
 └───────────┬─────────────┘
             ↓
 ┌─────────────────────────┐
 │ Reconciliation          │
 │ - Minimal changes list  │
 └───────────┬─────────────┘
             ↓
 ┌─────────────────────────┐
 │ Commit Phase            │
 │ - Apply to real DOM     │
 │ - Run effects/hooks     │
 └───────────┬─────────────┘
             ↓
 ┌─────────────────────────┐
 │ Browser Paint           │
 └─────────────────────────┘

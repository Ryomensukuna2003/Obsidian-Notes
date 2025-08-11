#### Nutshell 
Index is position given by array index and when CURD happens on components index changes by shifting and react thinks that new components have been mounted and re-renders all components  

#### Detailed
Using an **array index as a `key`** in React is an anti-pattern because it can break how React efficiently updates the DOM.

Here’s why:
1. **Keys are meant to identify stable elements**
    - React uses `key` to figure out which items in a list have changed, been added, or been removed.
    - If you use the index, the `key` changes when you reorder, insert, or remove items. This causes React to treat existing DOM nodes as entirely new elements.
2. **Unnecessary re-renders**
    - When items shift, React re-renders components unnecessarily because it thinks they’re different.
    - This can wipe out local component state (like an input field losing its typed value).
3. **Breaks state association**
    - State inside child components can get mixed up between list items because React reuses DOM nodes incorrectly.

**Example problem:**

```jsx
const [items, setItems] = useState(['A', 'B', 'C']);

{items.map((item, index) => (
  <input key={index} defaultValue={item} />
))}
```

If you insert `"X"` at the start, all inputs will shift, and `"A"` will keep the value typed into the `"B"` field.

**Better:**  
Use a **stable, unique id** for the `key`:

```jsx
{items.map(item => (
  <input key={item.id} defaultValue={item.name} />
))}
```

If you want, I can also explain when **using index is actually okay** — because it’s not _always_ bad.
## Native Web vs React

**Native Web DOM:**
- Has a **tree-like structure**.
- Some major flaws:
    - Slow update times.
    - Not optimized for modern interactive websites.

**React:**
- Think of it as a **tree-like structure** too, but:
    - Uses a **diffing algorithm**.
    - Updates **only what’s necessary** instead of re-rendering the whole DOM.
- We **don’t** create a new browser for React (too expensive to maintain).
- Instead:
    - Use **Web APIs** to grab an existing DOM element (like a `div` in the native browser tree).
    - React handles all **updates** and **deletions** efficiently.

```html
<html>
  <head>
  </head>
  <body>
    <div id="root">Not rendered</div>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script>
	    //React.createElement is a JS Object
		const heading = React.createElement(
			"div",            // <- tag name of element you want to create
			{id : "parent"} // <- attributes and we can also pass siblings if any
		);
		const root = ReactDOM.createRoot(document.getElementById("root"));
		root.render(heading);
    </script>
  </body>
</html>

```

#### 1. Difference between a Library and a Framework

|**Library**|**Framework**|
|---|---|
|Collection of reusable code/functions.|Complete structure for building applications.|
|You call the library when you need it (**You are in control**).|Framework calls your code at specific points (**Inversion of control**).|
|Example: React, Lodash.|Example: Angular, Django.|

---

#### 2. What is CDN? Why do we use it?

- **CDN** (Content Delivery Network) = a network of geographically distributed servers that deliver content faster by serving it from the nearest server.
- **Why use it?**
    - Faster load times.
    - Reduces load on your own server.
    - High availability (multiple copies around the world).
    - Caching improves performance.

#### 3. What is `crossorigin` in `<script>` tag?

- Attribute that defines **how browsers handle cross-origin requests** for the script.
- Common values:
    - `"anonymous"` → Sends no credentials (cookies/auth headers).
    - `"use-credentials"` → Sends credentials.
- In React CDN imports, it’s used so the browser can fetch the script from another domain while respecting **CORS policy**.    

#### 4. Difference between React and ReactDOM

|**React**|**ReactDOM**|
|---|---|
|Core library for building UI components, creating elements, managing state, etc.|Handles rendering React components to the DOM.|
|Platform-agnostic (can be used with React Native, etc.).|Browser-specific rendering methods.|
|Example: `React.createElement`|Example: `ReactDOM.createRoot` and `root.render`|

#### 5. Difference between `react.development.js` and `react.production.js` (CDN)

| **Development**                                              | **Production**                              |
| ------------------------------------------------------------ | ------------------------------------------- |
| Includes helpful warnings, error messages, and extra checks. | Minified and optimized — no extra warnings. |
| Larger file size, slower performance.                        | Smaller file size, faster performance.      |
| Used in local/dev environment.                               | Used in live/production apps.               |

#### 6. What is `async` and `defer` in `<script>`?

|Attribute|When Script Loads|When Script Executes|
|---|---|---|
|`async`|Downloads in parallel with HTML parsing.|Executes **as soon as it’s downloaded** (may pause HTML parsing).|
|`defer`|Downloads in parallel with HTML parsing.|Executes **after HTML parsing is complete** (before `DOMContentLoaded`).|

**Rule of thumb:**
- `async` → Good for scripts that don’t depend on DOM or other scripts.
- `defer` → Good for scripts that need the full DOM ready before execution.




#### Types of import/exports

Default import/export
- export default { componentName };
- import componentName from 'path/to/file'
- One file can have one default export

Named import/export
- export {componentName1, componentName2};
- import {componentName1, componentName2} from 'path/to/file'


```js
MAJOR.MINOR.PATCH
// Example: 4.17.15
```

#### What is this Major Minor Patch???

MAJOR (first number) → Breaking changes.
	Example: 4.x.x → 5.0.0 means something in the API changed in a way that might break existing code.

MINOR (second number) → New features, no breaking changes.
    Example: 4.17.x → 4.18.0.

PATCH (third number) → Bug fixes only.
    Example: 4.17.15 → 4.17.16.

#### Why ^ and ~ matter

- If you use **`^`**, npm will install newer **minor** and **patch** versions → safe according to SemVer, but you still need to trust that the package author follows SemVer (Semantic Versioning) properly.
- If you use **`~`**, npm will only install **patch** updates → lowest risk of accidental breakage.
- If you pin exact version (no `^` or `~`), you’ll **never get updates** unless you manually change it.


#### package.json

- **Purpose:** Declares your project’s **dependencies** and basic metadata.
- **Content:**
    - Name, version, scripts, and dependency ranges (e.g., `"^4.17.0"`).
    - It’s a **manifest** — describes what packages your project needs, not exactly what’s installed.
- **Editable:** You update it when adding/removing packages.
- **Version ranges:** Allows flexibility (e.g., `^`, `~`).

#### package-lock.json

- **Purpose:** Locks dependencies to **exact versions** for reproducible installs.
- **Content:**
    - Exact versions of every package **and their sub-dependencies**.
    - Hashes to verify package integrity.
- **Auto-generated:** Created/updated automatically by npm.
- **Version locking:** Ensures that if you and I run `npm install`, we get the **exact same versions**, even if the `package.json` allows a range.

package.json → **Shopping list**: “I need Milk for Chai. Milk can be any price between 26 and 36 price.”
package-lock.json → **Receipt**: “I bought 26 Milk.”


#### HMR -> Auto Refresh Components without full reload
Hot Module Replacement (HMR) in React is a feature that allows for modules to be updated in a running application without requiring a full page reload. This significantly improves the developer experience by preserving the application's state and reducing development time.

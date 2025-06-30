
---

## 📒 Notes on “Too many re-renders” in React

### 🔎 What happened?

* You called `setFlag(!flag)` directly **inside the body** of your React component:

  ```js
  function App() {
      const [flag, setFlag] = useState(true);
      setFlag(!flag); // ❌ causes infinite re-renders!
  }
  ```

* Every time React renders the component, it runs the function `App()`.

  * So `setFlag(!flag)` runs **on every render**, causing a state update immediately.

* A state update triggers a re-render → which again runs the component → which again calls `setFlag(!flag)` → **infinite loop** → React stops it with:

  > **Error: Too many re-renders. React limits the number of renders to prevent an infinite loop.**

---

### 🕰️ First vs. Later Renders

* **First render:**

  * React runs `App()` → sees `setFlag(!flag)` → updates state.
  * Because the state changed, React **schedules a re-render**.
* **Second render:**

  * React runs `App()` again → `setFlag(!flag)` runs again → updates state again.
* This repeats indefinitely → infinite renders → **error thrown**.

---

### ✅ Why is it wrong?

* The **component function body should be pure**: it should only calculate JSX output **based on the current props/state**.
* Calling `setState()` during rendering **violates this rule**, since it changes state during the calculation → immediate re-render loop.

---

### 🛠️ How to fix it?

Use `useEffect()` to perform state updates **after** the component is mounted. For example:

```js
import React, { useState, useEffect } from 'react';

function App() {
    const [flag, setFlag] = useState(true);

    useEffect(() => {
        setFlag(!flag); // ✅ runs only once after the first render
    }, []); // empty array: effect runs once on mount

    return (
        <div>
            <p>Flag: {flag.toString()}</p>
        </div>
    );
}
```

✅ **Now:**

* **First render:** React mounts → runs JSX → DOM updates → effect runs → `setFlag(!flag)` updates state → triggers one re-render.
* **Second render:** runs with the new flag → since the effect doesn’t run again (because dependencies are `[]`), **no infinite loop**.

---

### 🚨 Key takeaway:

> **Never call `setState()` directly inside the component body.**
>
> ➔ Only call it inside event handlers (like button clicks) or inside `useEffect()` if you need to update state after a render.

---


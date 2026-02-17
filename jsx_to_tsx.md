
---

# Migrating React JSX to TSX — Complete Developer Guide

Moving from JSX to TSX is like moving from plain JavaScript to JavaScript **with a strict rulebook**. JSX allows flexibility. TSX demands correctness. If you paste JSX directly into TSX, your code may still *look* fine, but TypeScript will start yelling at you.

Let’s break down **everything you must check and fix** before running JSX code inside TSX.

---

# 1. Understanding the Core Difference

## JSX

* Uses JavaScript
* No type safety
* Runtime errors possible

## TSX

* Uses TypeScript
* Compile-time type checking
* Better autocomplete, refactoring, and maintainability

👉 Think of JSX as writing notes casually.
👉 TSX is like writing notes with grammar checking enabled.

---

# 2. Install and Configure TypeScript Properly

Before fixing code, ensure TypeScript is configured.

## Required Packages

```
npm install --save-dev typescript @types/react @types/react-dom
```

## tsconfig.json Essentials

Make sure these exist:

```
{
  "compilerOptions": {
    "jsx": "react-jsx",
    "strict": true
  }
}
```

### Why this matters

Without proper config, TSX won’t understand React syntax or types.

---

# 3. Add Types to Props

## JSX Version

```
function Button(props) {
  return <button>{props.text}</button>;
}
```

## TSX Fix

```
type ButtonProps = {
  text: string;
};

function Button(props: ButtonProps) {
  return <button>{props.text}</button>;
}
```

### What You Fixed

* Defined expected prop structure
* Prevented accidental incorrect props

---

# 4. Add Types to Functional Components

### Recommended Modern Way

```
type Props = {
  title: string;
};

const Header = ({ title }: Props) => {
  return <h1>{title}</h1>;
};
```

---

# 5. Fix useState Types

This is the **most common error** when migrating JSX.

## JSX

```
const [count, setCount] = useState(0);
```

Usually works automatically, but fails in complex cases.

---

## When Type Must Be Explicit

### Example — Null or Undefined

```
const [user, setUser] = useState<User | null>(null);
```

---

## Example — Arrays

```
const [items, setItems] = useState<string[]>([]);
```

---

## Example — Objects

```
type User = {
  name: string;
  age: number;
};

const [user, setUser] = useState<User>({
  name: "",
  age: 0
});
```

---

# 6. Type Event Handlers

JSX allows sloppy event handling. TSX does not.

---

## JSX

```
const handleChange = (e) => {
  console.log(e.target.value);
};
```

---

## TSX Fix

```
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  console.log(e.target.value);
};
```

---

## Common Event Types

### Input Change

```
React.ChangeEvent<HTMLInputElement>
```

### Form Submit

```
React.FormEvent<HTMLFormElement>
```

### Button Click

```
React.MouseEvent<HTMLButtonElement>
```

---

# 7. Add Types to useRef

## JSX

```
const inputRef = useRef(null);
```

## TSX Fix

```
const inputRef = useRef<HTMLInputElement>(null);
```

---

# 8. Fix Function Return Types (Optional but Recommended)

```
function sum(a: number, b: number): number {
  return a + b;
}
```

This improves maintainability.

---

# 9. Type Children Properly

## JSX

```
function Card({ children }) {
  return <div>{children}</div>;
}
```

---

## TSX Fix

```
type CardProps = {
  children: React.ReactNode;
};

function Card({ children }: CardProps) {
  return <div>{children}</div>;
}
```

---

# 10. Fix Inline Object Types

## JSX

```
const user = {
  name: "Hari",
  age: 20
};
```

---

## TSX Recommended

```
type User = {
  name: string;
  age: number;
};

const user: User = {
  name: "Hari",
  age: 20
};
```

---

# 11. Handle Optional Props

```
type Props = {
  title?: string;
};
```

Optional props prevent runtime crashes.

---

# 12. Fix Default Props

### Use Default Values Instead

```
const Header = ({ title = "Default" }: Props) => {
  return <h1>{title}</h1>;
};
```

---

# 13. Handle API Data Types

Never trust API responses blindly.

```
type User = {
  id: number;
  name: string;
};

const fetchUsers = async (): Promise<User[]> => {
  const res = await fetch("/api/users");
  return res.json();
};
```

---

# 14. Fix Map Iteration Types

## JSX

```
users.map(user => <div>{user.name}</div>);
```

---

## TSX Fix

```
users.map((user: User) => <div key={user.id}>{user.name}</div>);
```

---

# 15. Fix Implicit Any Errors

TypeScript strict mode disallows:

```
function greet(name) {}
```

Fix:

```
function greet(name: string) {}
```

---

# 16. Use Interfaces or Types Consistently

Both work.

```
interface User {
  name: string;
}
```

OR

```
type User = {
  name: string;
};
```

---

# 17. Fix External Library Types

Sometimes JSX libraries lack TypeScript support.

Install types:

```
npm install @types/library-name
```

---

# 18. Fix Context API Types

```
type ThemeContextType = {
  theme: string;
  setTheme: (theme: string) => void;
};
```

---

# 19. Avoid Using "any"

Bad:

```
const data: any
```

Better:

```
unknown
```

or proper type definition.

---

# 20. Fix Enum or Constant Types

```
enum Status {
  Loading,
  Success,
  Error
}
```

---

# 21. Fix Custom Hooks Types

```
function useUser(): User {
  return user;
}
```

---

# 22. Fix Third-Party Component Props

Check library documentation for type definitions.

---

# 23. Fix CSS Modules Types

If using CSS modules:

```
import styles from "./style.module.css";
```

You may need:

```
declare module "*.module.css";
```

---

# 24. Fix Async Function Return Types

```
const fetchData = async (): Promise<void> => {}
```

---

# 25. Fix Spread Props Issues

```
<Component {...props} />
```

Ensure props have defined type.

---

# 26. Enable Strict Mode (Highly Recommended)

```
"strict": true
```

Catches hidden bugs early.

---

# 27. Common Migration Workflow

### Step 1

Rename file

```
Component.jsx → Component.tsx
```

### Step 2

Fix red TypeScript errors one by one.

### Step 3

Add interfaces and types gradually.

---

# 28. Migration Tips (Real Developer Advice)

✔ Start with smaller components
✔ Type API responses first
✔ Avoid rushing to remove all "any" initially
✔ Use TypeScript inference where possible

---

# 29. Useful Helper Types

### Partial

```
Partial<User>
```

### Pick

```
Pick<User, "name">
```

### Record

```
Record<string, number>
```

---

# 30. Common Beginner Mistakes

❌ Typing everything manually
❌ Overusing "any"
❌ Forgetting null checks
❌ Ignoring event types

---

# Final Migration Checklist ✅

Before running TSX:

* Props typed
* State typed
* Event handlers typed
* Refs typed
* API responses typed
* Children typed
* Optional props handled
* No implicit any
* External types installed
* tsconfig configured

---

# When JSX Code Usually Works Without Changes

* Static UI components
* No props/state
* No events
* No external data

---

# Conclusion

Migrating JSX to TSX improves:

* Code safety
* Debugging speed
* Developer productivity
* Large project maintainability

It feels strict initially, but once you get used to it, TSX actually **reduces bugs dramatically**.

---


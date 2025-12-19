
---

Let’s assume a **standard React + Vite project**.

### BASE FOLDER STRUCTURE

```
my-app/
├─ index.html
├─ package.json
├─ vite.config.js
├─ src/
│  ├─ main.jsx
│  ├─ App.jsx
│  └─ components/
├─ public/
│  ├─ logo.png
│  └─ verify.txt
├─ random.html
├─ notes.txt
├─ test/
└─ another-app/
```

Now, when you run:

```
npm run build
```

Vite does **NOT scan everything**.
It follows a **very strict, limited range**.

---

## THE EXACT RANGE VITE BUILD CARES ABOUT

Vite build starts from **one single entry point**:

```
index.html
```

From there, it does **dependency walking**.

### STEP 1: index.html

Vite reads `index.html` and looks for:

```
<script type="module" src="/src/main.jsx"></script>
```

Anything referenced here becomes part of the build.

If a file is **not reachable from index.html**, it is invisible.

---

### STEP 2: src/ (ONLY what is imported)

Inside `src/`, Vite includes **only files that are imported**.

Example:

```
src/
├─ main.jsx        ← included (entry)
├─ App.jsx         ← included (imported)
├─ components/
│  ├─ Header.jsx   ← included (imported)
│  └─ Footer.jsx   ← ❌ ignored if never imported
├─ utils.js        ← ❌ ignored if unused
└─ temp.js         ← ❌ ignored
```

Rule:

• Imported → included
• Not imported → ignored

Vite does **not** include unused files just because they exist.

---

### STEP 3: Assets referenced by code

If your JS or CSS references assets:

```
import img from './image.png';
```

or

```
background: url('./bg.jpg');
```

Those assets are included.

If an asset exists but is never referenced:

❌ ignored

---

### STEP 4: public/ (special case)

Everything inside `public/` is included **no questions asked**.

```
public/
├─ logo.png        ← included
├─ verify.txt      ← included
├─ anything.pdf    ← included
```

This folder is copied directly into `dist/`.

Vite does not check usage here.

---

## WHAT IS COMPLETELY OUT OF RANGE

These are **100% ignored** by Vite build:

```
random.html
notes.txt
test/
another-app/
```

Why?

• Not referenced by index.html
• Not imported by src
• Not inside public

So they never reach `dist/`.

---

## MULTIPLE APPS INSIDE ONE PROJECT (YOUR QUESTION)

Example:

```
my-app/
├─ index.html          ← main app
├─ src/
├─ public/
├─ admin/
│  ├─ index.html
│  └─ src/
├─ dashboard/
│  ├─ index.html
│  └─ src/
```

What happens?

By default:

• ONLY the root `index.html` is used
• `admin/` and `dashboard/` are ❌ ignored

Unless you **explicitly configure** Vite for multiple entry points.

Vite does NOT auto-detect multiple apps.

---

## SIMPLE RULE (THIS IS THE RANGE)

Vite build range is:

```
index.html
   ↓
imports inside src/
   ↓
assets referenced by those imports
   +
everything inside public/
```

Nothing else exists.

---

## ONE-LINE SUMMARY (NO THEORY)

If a file is:
• reachable from index.html → included
• inside public/ → included
• otherwise → ignored

That’s the entire range.

---


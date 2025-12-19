
# How File Serving Works in React + Vite (Dev vs Build vs Deployment)

This note explains how files are served in a React app built with Vite, why some files are accessible locally but disappear in production, and what exactly goes into the build. This is practical, real behavior — not theory.

---

## 🧠 Core Idea: Who Is Serving the File?

Files don’t magically become accessible just because they exist in the project directory.  
A file is accessible only if a **server explicitly serves it**.

There are **two distinct phases**:

1. **Development (localhost)**  
2. **Production (npm run build + deployment)**

The rules change completely between these phases.

---

## 🛠 Development Phase (localhost)

When you run:

```bash
npm run dev
````

You are **not running a plain static server**.
Instead, you are running the **Vite development server**.

The Vite dev server does **not** serve the entire project folder. It serves only:

* `index.html`
* JS/CSS modules imported by your code
* Files inside the `public/` folder

That’s it.

### ❌ Files Parallel to index.html Don’t Work

Example:

```
public/
src/
about.html            ← ❌ NOT served
index.html
```

If you open:

```
http://localhost:5173/about.html
```

It **will not work** because Vite does not expose arbitrary root files.

> Vite is a **module server**, not a generic file server.

---

## 📍 URLs Without Extensions

Paths like:

```
http://localhost:5173/blog
```

do **not** require a file called `blog.html`.

Instead:

* The browser requests `/blog`
* Vite returns `index.html`
* React Router handles the route
* React renders the `/blog` page

So extension-less URLs are **client-side routing**, not file serving.

---

## 📁 The `public/` Folder (Important)

The `public/` folder is special.
Anything in `public/` is served **as-is** both in dev and after build.

Examples:

| File in public/           | Accessible URL                 |
| ------------------------- | ------------------------------ |
| `public/image.png`        | `https://.../image.png`        |
| `public/loaderio-xxx.txt` | `https://.../loaderio-xxx.txt` |

✔ No bundling
✔ No hashing
✔ Works in dev & production

This is why Loader.io verification files must go inside `public/`.

---

## 📦 Production Build (npm run build)

When you run:

```bash
npm run build
```

Vite **does not deploy your project folder**.
Instead, it generates a new folder called `dist/` containing production-ready files.

### Only these go into `dist/`:

1. `index.html` (processed)
2. JS/CSS files that are:

   * imported from `src/`
   * reachable from `index.html`
3. Assets referenced by JS/CSS
4. Everything inside `public/`

**Nothing else is included.**

---

## ❌ Why Files Parallel to index.html Disappear

If you place:

```
about.html
```

parallel to `index.html` (outside public/) and deploy, it **will not appear online**.
Reason:

* Vite doesn’t copy arbitrary root files
* They are not imported anywhere
* They are not inside `public/`
* They are skipped during build
* Vercel deploys only `dist/`

So those files never exist after deployment.

---

## 📤 Why public Files Work After Deployment

During build:

```
public/  →  dist/
```

Everything inside `public/` is copied directly into `dist/` without change.

So:

```
public/file.txt  →  dist/file.txt  → deployed
```

This behavior is intentional.

---

## 🧠 What Build “Cares About”

Vite build only includes what is **necessary to run the app**:

✔ Imported modules
✔ Referenced assets
✔ Static files in `public/`

Everything else is ignored.

---

## ❌ What Vite Build Skips

By default, Vite build ignores:

* Random HTML files outside `public/`
* Files not imported anywhere
* Dev-only files
* Test files
* Notes, docs, configs

Unless:

* The file is inside `public/`
* OR it is imported via JS or CSS

---

## 🤔 Why This Design Exists

This design makes sense because:

* React apps are Single Page Applications
* Routing is handled by JavaScript
* Production servers should be minimal
* Shipping unnecessary files is wasteful or unsafe

Vite enforces this discipline.

---

## ⚡ Final Mental Model

Remember this:

```
DEV MODE
→ Vite serves modules dynamically

BUILD MODE
→ Only what reaches dist/ exists

PUBLIC FOLDER
→ Always copied, always accessible
```

If you remember just this sentence, everything clicks.

---

## 📌 Next You Can Learn

* Differences in Next.js file handling
* How React Router + Vercel rewrites work
* API routes vs static files
* Why serverless platforms behave differently under load

Just ask 🙂



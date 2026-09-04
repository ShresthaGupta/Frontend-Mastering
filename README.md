# Frontend-Mastering
**npm** installs/manages packages; **npx** executes a package/CLI without needing to install it globally.
**Components**

In React, components are primarily meant to render UI based on data (state and props). 
However, sometimes a component needs to do things that reach outside of this rendering cycle. We call these "side effects".

The useEffect hook lets us perform these side effects in function components.
```javascript
import { useState, useEffect } from 'react'

function App() {
  const [count, setCount] = useState(0)

  // 1. We call useEffect and pass it a function
  useEffect(() => {
    // 2. This is the "side effect" - talking to the browser API
    document.title = `You clicked ${count} times`;
    
  }, [count]); // 3. The Dependency Array
```

> How it works:
**The Effect Function**: The first argument is the function where you put your side-effect logic (document.title = ...). React will run this function after it finishes rendering the component to the screen.
**The Dependency Array**: The second argument [count] is extremely important. It tells React: "Only run this effect function again if the count variable has changed since the last render."

> If you didn't include the array, the effect would run after every single render, which can be inefficient.

> If you passed an empty array [], the effect would only run once when the component first appears on the screen (useful for initial data fetching).

Other common simple use cases for useEffect:

Data Fetching: Hitting an API (like fetching a list of users from a database) when the component first loads useEffect(..., []).
Setting up subscriptions/timers: Creating a setInterval or setTimeout.

**Vite vs Next**
Vite is a lightning-fast build tool for rendering React apps entirely in the browser (Client-Side), whereas, 
Next.js is a full-stack framework that gives you server-side rendering, file-based routing, and backend capabilities right out of the box.

**Vite Commands to build the boilerplate app**
npm create vite@latest

> dev vs build
`npm run dev` and `npm run build` are two different jobs. Dev serves your source **as modules** so edits show up fast. Build **type-checks**, then **bundles** a static site you can host.

## `npm run dev`

`package.json` maps that script to Vite:

```6:8:c:\Learning\upskill.co\react_vite\vite-project\package.json
    "dev": "vite",
    "build": "tsc -b && vite build",
```

What happens:

1. **Vite starts a local server** (usually `http://localhost:5173`).
2. The browser requests **`index.html`**. That file is the real entry, not `main.tsx`.
3. HTML loads `/src/main.tsx` as an ES module:

```11:11:c:\Learning\upskill.co\react_vite\vite-project\index.html
    <script type="module" src="/src/main.tsx"></script>
```

4. The browser then asks Vite for `main.tsx`, `App.tsx`, CSS, images, `react`, etc. Vite **transforms on demand** (TS/JSX → JS the browser can run). It does **not** pack the whole app into one file first.
5. **`@vitejs/plugin-react`** (from `vite.config.ts`) turns JSX into `React` calls and enables **Fast Refresh** (HMR): save a component, Vite patches that module instead of a full reload.
6. Files in **`public/`** are served as-is (`/favicon.svg`). Imports from **`src/`** are processed.

So in dev, the “app” is: **HTML → Vite middleware → your `src` files, compiled as you request them.**

---

## `npm run build`

That script is **two steps**, in order: `tsc -b && vite build`.

### Step 1: `tsc -b`

TypeScript project references (`tsconfig.json` → `tsconfig.app.json` + `tsconfig.node.json`) **type-check** the app and `vite.config.ts`.  
`noEmit: true` means it **does not output JS**. If types fail, `&&` stops and Vite never runs.

### Step 2: `vite build`

Vite (in this version, via its bundler) produces a **production** folder, usually **`dist/`**:

- Walks from `index.html` → `main.tsx` → everything imported.
- Compiles TS/JSX, minifies JS/CSS, tree-shakes unused code.
- Hashes filenames (`App-a1b2c3.js`) so browsers cache safely.
- Copies `public/` into `dist/`.
- Rewrites `index.html` to load those hashed files (no `/src/main.tsx` in production).

Result: static files a normal web server can host. No Vite process required at runtime.

`npm run preview` serves `dist/` locally so you can check the **built** app, not the dev server.

---

## Dev vs build (why both exist)

| | `npm run dev` | `npm run build` |
|---|---|---|
| Goal | Fast feedback | Ship to production |
| Bundling | Almost none; native modules | Full bundle + minify |
| TypeScript | Vite transforms; **no full `tsc` gate** | **`tsc -b` runs first** |
| Output | Nothing in `dist/`; RAM + cache | `dist/` |

That is why a type error can still “run” in dev, then fail on `npm run build`.

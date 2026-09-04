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

## dev vs build
`npm run dev` and `npm run build` are two different jobs. Dev serves your source **as modules** so edits show up fast. Build **type-checks**, then **bundles** a static site you can host.

Sure — here's the **noob-friendly version**. No jargon unless absolutely necessary.

### `npm run dev` — while I'm developing

1. **Starts a local server** on your computer so you can open your React app in the browser.
2. **Opens `index.html`**, which tells the browser where your React application starts (`main.tsx`).
3. **Loads your React files when the browser needs them** — it doesn't prepare the whole app beforehand.
4. **Watches your files for changes**. When you save something, Vite notices it.
5. **Quickly updates the browser** with your changes, so you don't have to manually refresh every time.

**In one line:**
👉 `npm run dev` = **Start the app + let me develop it + show my changes immediately.**

---

### `npm run build` — when I'm ready to deliver

1. **Checks your code for TypeScript errors** first.
2. If everything is okay, **Vite collects all the code your app needs**.
3. **Converts and optimizes the code** so browsers can run it efficiently.
4. **Creates the final production files** inside the `dist/` folder.
5. Those files can then be **uploaded to a server/hosting platform** for real users.

**In one line:**
👉 `npm run build` = **Check my code + prepare everything + create the final version for users.**


## only 1 jsx/tsx element can be returned.
**we can keep more elements inside a div element ultimately returning single elements**

> Unlike Create React App, Vite keeps things simpler and more visible.
In a Vite project, `index.html` directly points to your main file, like `main.jsx` or `main.tsx`. During development, Vite uses modern JavaScript modules to load the files when they are needed instead of first bundling the entire application. This makes the development server start very quickly and lets us see changes almost immediately.




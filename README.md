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

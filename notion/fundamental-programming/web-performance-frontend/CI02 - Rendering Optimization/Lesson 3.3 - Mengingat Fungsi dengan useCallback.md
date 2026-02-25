💡 CI02-3.3: Stabilizing Functions with useCallback

Outline:

- **The Metric & The Bottleneck (Presentation):** The `React.memo` and referential equality problem for functions.
- **The Optimization Technique (Practice):** Using the `useCallback` hook to memoize a function definition.
- **Your Performance Mission (Production):** Combining `useCallback` and `React.memo` to solve a classic re-render issue.

📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** We've learned that `React.memo` is powerful, but it has a weakness: it doesn't work if its props are functions that are redefined on every render. Today's goal is to solve that exact problem using the **`useCallback` hook**.
    
- **Identifying the Problem:** The bottleneck is the re-creation of functions during rendering.
    
    ```tsx
    function Parent() {
      const [count, setCount] = useState(0);
      const handleClick = () => console.log('Clicked!'); // New function every render
      return <MemoizedChild onClick={handleClick} />;
    }
    
    ```
    
    Even though `<MemoizedChild>` is wrapped in `React.memo`, it will re-render every time `Parent` does. Why? Because on each render of `Parent`, a brand new `handleClick` function is created. `React.memo` compares the old `handleClick` reference with the new one, finds they are different, and proceeds with the re-render. Our memoization is defeated.
    

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** The `useCallback` hook gives us a way to tell React: "Don't re-create this function on every render. Give me back the _exact same_ function instance as last time, unless its dependencies have changed." This provides a stable reference that `React.memo` can rely on.
    
- **Secure Code Implementation:**`useCallback` takes two arguments:
    
    1. The function definition you want to memoize.
    2. A dependency array (just like `useEffect`).
    
    ```tsx
    import { useState, useCallback } from 'react';
    
    function Parent() {
      const [count, setCount] = useState(0);
    
      // Tell React to memoize this function.
      // The dependency array [] is empty, so this function will NEVER be re-created.
      const handleClick = useCallback(() => {
        console.log('Clicked!');
      }, []); // Dependency array
    
      return <MemoizedChild onClick={handleClick} />;
    }
    
    ```
    
    Now, when `Parent` re-renders, React sees the `useCallback`. It checks the dependency array, sees that it's empty and hasn't changed, and returns the _identical_ `handleClick` function instance from the previous render. `React.memo` in the child compares the references, finds they are the same, and correctly skips the re-render.
    

### 🧠 **Real-World Case Study: "The List of Buttons"**

- **The Scenario:** An application renders a long list of items. Each item has a delete button. The `<ListItem>` component is wrapped in `React.memo`. `<ListItem data={item} onDelete={handleDelete} />`
- **The Problem:** When the user adds a new item to the list, _every single_ `<ListItem>` in the list re-renders, causing a janky UI update.
- **The Analysis:** The Profiler shows that the `onDelete` prop is changing for every item. The `handleDelete` function was defined inside the parent component's render function, and it needed the `itemId` to work. `const handleDelete = (itemId) => { ... }`
- **The Fix:** The team refactored the parent. They used `useCallback` to memoize the delete handler. `const handleDelete = useCallback((itemId) => { ... }, []);` Now, a single, stable `handleDelete` function is created. It is passed to all `<ListItem>` components. When a new item is added, the parent re-renders, but the `handleDelete` prop passed to the existing items is referentially identical to the previous render. `React.memo` now works as expected, and only the new item is rendered.

### 🤔 **Reflective Questions**

1. What happens if you omit the dependency array from `useCallback`? How does it behave differently from a plain function definition?
2. If a function inside `useCallback` needs to access the latest component state, what must you include in the dependency array? What is the risk if you forget?
3. When is it **not** necessary to use `useCallback`? (Hint: think about components that are not memoized).

### 📚 **Further Reading**

- **React Docs:** [`useCallback`](https://react.dev/reference/react/useCallback)
- **In-depth Guide:** [A complete guide to useCallback](https://www.google.com/search?q=https://www.developerway.com/posts/react-re-renders-guide%23usecallback)
- **Auditing Tool:** React DevTools Profiler

### 📝 **Mini Task (Production)**

- **Your Mission:** You are given an app where `React.memo` is failing because of an unstable function prop. Your job is to stabilize it.
- **Link to Sandbox:** [Unstable Function Prop Sandbox](https://www.google.com/search?q=https://codesandbox.io/s/ci02-memo-task-2-forked-qgypmf)
- **Your Deliverable:**
    1. Fork the sandbox. Notice that clicking the "Increment" button in the parent still causes the `Header` to re-render, even though it's memoized.
    2. In `App.js`, import the `useCallback` hook.
    3. Wrap the `handleLogin` function definition in `useCallback` with an empty dependency array.
    4. Verify your fix. Now, when you click "Increment," the "Rendering Header..." message should no longer appear in the console. You have successfully combined `memo` and `useCallback`.
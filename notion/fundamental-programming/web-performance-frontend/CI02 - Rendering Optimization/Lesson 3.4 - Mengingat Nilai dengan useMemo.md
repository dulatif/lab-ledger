💡 CI02-3.4: Avoiding Expensive Calculations with useMemo

Outline:

- **The Metric & The Bottleneck (Presentation):** The problem of slow, repetitive calculations blocking the main thread.
- **The Optimization Technique (Practice):** Using the `useMemo` hook to memoize the _result_ of a calculation.
- **Your Performance Mission (Production):** Applying `useMemo` to fix a component with a slow render.

📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our final memoization tool is **`useMemo`**. Its purpose is different. While `useCallback` memoizes a function, `useMemo` memoizes a **value**. Our goal is to prevent expensive, time-consuming calculations from running on every single render.
- **Identifying the Problem:** The bottleneck is **synchronous, expensive computation during render**. Imagine a component that needs to filter or sort a very large array, or perform a complex mathematical calculation to derive some data. If this calculation is done directly in the component body, it will re-run every time the component re-renders, for any reason. This can block the main thread, freeze the UI, and lead to a terrible user experience. Our metric here is the "self-duration" of a component in the Profiler.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** The `useMemo` hook memoizes the _return value_ of a function. It takes a function and a dependency array. React will only re-run the function (and thus re-calculate the value) if one of the dependencies has changed. On all other re-renders, it will return the cached value from the last time it ran.
    
- **`useCallback` vs. `useMemo`**
    
    - `useCallback(fn, deps)` returns a memoized **function**.
    - `useMemo(() => fn(value), deps)` calls the function and returns its memoized **result**.
- **Secure Code Implementation:**
    
    ```
    // BEFORE: This slow function runs on EVERY render, even if `todos` hasn't changed.
    function TodoList({ todos, filter }) {
      const visibleTodos = filterTodos(todos, filter); // Assume this is very slow
      return <ul>...</ul>;
    }
    
    // AFTER: The calculation only runs when `todos` or `filter` changes.
    import { useMemo } from 'react';
    
    function TodoList({ todos, filter }) {
      // The result of filterTodos is memoized.
      const visibleTodos = useMemo(() => {
        return filterTodos(todos, filter); // The expensive calculation
      }, [todos, filter]); // The dependencies
    
      return <ul>...</ul>;
    }
    
    ```
    

### 🧠 **Real-World Case Study: "The Laggy Chart"**

- **The Scenario:** A financial dashboard has a chart component that takes a huge array of raw transaction data and transforms it into a specific format needed by the charting library.
- **The Problem:** Every time the user hovers over a different part of the dashboard, a `hovered` state changes in the parent component, which causes the chart to re-render. The data transformation logic, which takes ~300ms, re-runs every time, causing the entire UI to lag on every mouse movement.
- **The Analysis:** The Profiler shows the `<Chart>` component has a very high "self-duration" of 300ms on every commit, even when its `transactions` prop isn't changing.
- **The Fix:** The developer wrapped the data transformation logic in `useMemo`. `const chartData = useMemo(() => transformData(transactions), [transactions]);`
- **The Takeaway:** Now, the 300ms transformation only runs when the `transactions` prop itself actually changes. For all other re-renders (like the hover state update), `useMemo` returns the cached `chartData` instantly. The Profiler confirms the component's self-duration is now <1ms on hover, and the UI is perfectly smooth.

### 🤔 **Reflective Questions**

1. Can you achieve the same result as `useCallback(fn, deps)` by using `useMemo(() => fn, deps)`? Why are both hooks provided? (Hint: readability and intent).
2. What is the danger of providing an incorrect dependency array to `useMemo`? What kind of bugs could this cause?
3. Like `React.memo`, `useMemo` is not free. What is the overhead, and why shouldn't you wrap every single calculation in `useMemo`?

### 📚 **Further Reading**

- **React Docs:** [`useMemo`](https://react.dev/reference/react/useMemo)
- **When to useMemo:** [A detailed article by Kent C. Dodds](https://kentcdodds.com/blog/usememo-and-usecallback)
- **Auditing Tool:** React DevTools Profiler

### 📝 **Mini Task (Production)**

- **Your Mission:** You are given a component that is artificially slow. Your task is to use `useMemo` to make it fast.
- **Link to Sandbox:** [Slow Calculation Sandbox](https://www.google.com/search?q=https://codesandbox.io/s/ci02-memo-task-3-forked-v6g7sc)
- **Your Deliverable:**
    1. Fork the sandbox. Notice that typing in the input field is extremely slow and laggy. This is because the `filteredUsers` calculation is re-running on every keystroke.
    2. In `App.js`, import the `useMemo` hook.
    3. Wrap the `users.filter(...)` logic inside a `useMemo` hook.
    4. Provide the correct dependency array so the filter only re-runs when it absolutely needs to.
    5. Verify your fix. Typing in the input should now be instantaneous and smooth.
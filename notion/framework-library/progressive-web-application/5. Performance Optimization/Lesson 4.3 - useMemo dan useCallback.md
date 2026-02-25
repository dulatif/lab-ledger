# 💡 AM01.4.3: The Memory Banks: Optimizing with useMemo and useCallback

**Outline:**

- **The Problem with Referential Equality (Presentation):** Understanding why `React.memo` sometimes isn't enough.
- **Memoizing Values and Functions (Practice):** Clear examples of `useMemo` for expensive calculations and `useCallback` for stable functions.
- **Stabilizing Your Props (Production):** Applying `useCallback` to fix a broken memoization.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Problem with Referential Equality**

You've wrapped a child component in `React.memo`, but the Profiler shows it's still re-rendering. What's going on? The problem is likely **referential equality**.

In JavaScript, non-primitive types like objects (`{}`), arrays (`[]`), and functions (`() => {}`) are compared by _reference_, not by value. This means that `{} === {}` is always `false`, because they are two different objects in memory.

Every time a parent component re-renders, it re-creates any functions or objects defined inside it. This means even if a function or object is _conceptually_ the same, it gets a new reference in memory. When this is passed as a prop to a memoized child, `React.memo`'s shallow comparison sees a new reference and says, "The prop changed!" It then re-renders the child, defeating our optimization.

The solution is two hooks: `useCallback` and `useMemo`. They allow us to memoize functions and values within a component, ensuring they keep the same reference between renders unless their own dependencies change.

- `useCallback(fn, deps)`: Memoizes a **function**. It returns the same function reference as long as the dependencies (`deps`) haven't changed.
- `useMemo(createFn, deps)`: Memoizes a **value**. It runs the `createFn` and returns its result. It will only re-run the function if the dependencies (`deps`) have changed.

### **Part 2: Practice - Memoizing Values and Functions**

**Use Case 1: `useMemo` for expensive calculations** Without `useMemo`, `getVisibleTodos` would run on _every_ render, even if `todos` and `filter` haven't changed.

```ts
function TodoList({ todos, filter }) {
  // This is a potentially slow calculation
  const visibleTodos = getVisibleTodos(todos, filter);
  // ...
}

// With useMemo, it only re-calculates when needed
import { useMemo } from 'react';

function TodoList({ todos, filter }) {
  const visibleTodos = useMemo(() => {
    return getVisibleTodos(todos, filter);
  }, [todos, filter]); // Dependency array
  // ...
}
```

**Use Case 2: `useCallback` to stabilize props for a memoized child**`React.memo` on `<MyButton>` will fail without `useCallback`.

```ts
function Parent() {
  const [count, setCount] = useState(0);

  // This function is re-created on every render of Parent
  const handleClick = () => {
    console.log('Button clicked!');
  };

  return <MyButton onClick={handleClick} />; // MyButton re-renders every time
}

// With useCallback, MyButton's memoization now works
import { useCallback } from 'react';

function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log('Button clicked!');
  }, []); // Empty dependency array means this function is created only once

  return <MyButton onClick={handleClick} />;
}
```

## **🧠 Real-World Case Study: "The Un-memoizable Button"**

- **The Problem:** A team had a `<DataTable>` component that rendered hundreds of rows. Each `<Row>` had an `<IconButton onClick={...}>` prop. They had memoized `<Row>` and `<IconButton>`, but the Profiler showed that every row and every button re-rendered when the table was sorted.
- **The Diagnosis:** They looked at the parent `<DataTable>` component. The `onClick` handler for the button was defined as an arrow function directly in the JSX: `<IconButton onClick={() => deleteRow(row.id)} />`.
- **The Fix:** This created a new function on every single render for every single row. They refactored the parent to use `useCallback` for the delete handler, which took the `id` as an argument. The child `<IconButton>` was changed to call this stable function.
- **The Result:** The `onClick` prop was now stable between renders. After sorting, the Profiler showed that zero rows re-rendered. The sorting operation became visually instantaneous.

## 🤔 **Reflective Questions**

1. What is the key difference between `useMemo` and `useCallback`? How can you create `useCallback` using `useMemo`?
2. What does the "dependency array" (the second argument) do for these hooks? What happens if you forget to include a dependency?
3. Like `React.memo`, these hooks have a cost. When is it _not_ a good idea to use `useMemo` or `useCallback`?

## 📚 **Further Reading**

- **React Docs:** [`useCallback`](https://react.dev/reference/react/useCallback)
- **React Docs:** [`useMemo`](https://react.dev/reference/react/useMemo)

## 📝 **Mini Task (Production)**

Let's fix the broken memoization.

1. Find a memoized child component in your app that accepts a function as a prop (e.g., an `onClick` handler).
2. In the parent component that renders it, find the definition of that handler function.
3. Import `useCallback` and wrap the handler function in it. Provide the correct dependency array.
4. Profile the interaction again. If the function was the only prop breaking the memoization, your child component should now correctly skip re-rendering.
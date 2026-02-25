# 💡 AT01.5.1: The Great Separation: Smart vs. Dumb Components

**Outline:**

- **The Single Responsibility Principle (Presentation):** Applying a core software design principle to your React components.
- **The Refactor (Practice):** A hands-on example of splitting a monolithic component into two focused ones.
- **The Reusable View (Production):** Applying the pattern to build a truly reusable presentation layer.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Single Responsibility Principle**

Team, one of the most important principles for creating maintainable, scalable software is the **Single Responsibility Principle (SRP)**. It states that a class or module should have only one reason to change. We can apply this directly to our React components to create a clean, predictable architecture.

This leads to the **Smart and Dumb Component** pattern (also known as Container/Presentational).

1. **Smart Components (Containers):** Their only job is to manage logic. They know _how things work_. They fetch data, connect to Redux stores, and handle state. They rarely contain any significant styling or markup. They pass data and callbacks down as props.
2. **Dumb Components (Presentational):** Their only job is to render UI. They know _how things look_. They receive all their data via props and communicate with the outside world by calling function props. They are often pure functions of their props and are highly reusable.

This separation is critical for scalability. It allows you to change your data-fetching logic without touching your UI, and vice versa. It makes your presentational components incredibly easy to test and document, often in isolation using tools like Storybook.

### **Part 2: Practice - The Refactor**

Let's see this in action. Here's a common "monolithic" component that mixes concerns.

**Before: One component does everything**

```tsx
// UserProfile.tsx
import React, { useState, useEffect } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { fetchUser, updateUser } from './userSlice';

export const UserProfile = ({ userId }) => {
  const user = useSelector((state) => state.user.data);
  const status = useSelector((state) => state.user.status);
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(fetchUser(userId));
  }, [dispatch, userId]);

  if (status === 'loading') return <p>Loading...</p>;

  return (
    <div className="profile-card">
      <img src={user.avatar} alt={user.name} />
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
};
```

**After: Separating Logic from Presentation**

**The Smart Component (Container):**

```tsx
// UserProfileContainer.tsx
import React, { useEffect } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { fetchUser } from './userSlice';
import { UserProfileView } from './UserProfileView';

export const UserProfileContainer = ({ userId }) => {
  const user = useSelector((state) => state.user.data);
  const status = useSelector((state) => state.user.status);
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(fetchUser(userId));
  }, [dispatch, userId]);

  return <UserProfileView user={user} status={status} />;
};
```

**The Dumb Component (Presentational):**

```tsx
// UserProfileView.tsx
import React from 'react';

// This component is now pure. It only knows how to render props.
export const UserProfileView = ({ user, status }) => {
  if (status === 'loading') return <p>Loading...</p>;
  if (!user) return null;

  return (
    <div className="profile-card">
      <img src={user.avatar} alt={user.name} />
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
};
```

Now, `UserProfileView` is completely decoupled from Redux and data fetching. We can reuse it anywhere, and test it just by passing it different props.

## 🧠 **Real-World Case Study: "The Storybook Revolution"**

- **The Problem:** A design team complained that their component library was hard to work with. Every component was tangled with data-fetching logic, so they couldn't see all of its visual states (loading, error, empty, full) without running the entire application.
- **The Solution:** The engineering team rigorously applied the Smart/Dumb pattern. They refactored every component into a "Container" and a "View". The "View" components were pure and presentational.
- **The Result:** They were able to put all their "View" components into Storybook, an isolated UI development environment. Designers could now browse every component and interactively test every possible visual state by tweaking its props. This dramatically improved collaboration between design and engineering and resulted in a more robust and consistent UI.

## 🤔 **Reflective Questions**

1. What are the benefits of this pattern when it comes to testing?
2. With the rise of React hooks, some argue this pattern is less necessary. What's the counter-argument? (Hint: even with hooks, a component can still have multiple responsibilities).
3. How does this pattern help with onboarding new developers to a large codebase?

## 📚 **Further Reading**

- **Dan Abramov:** [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0) (The original article that popularized the pattern).
- **React Docs:** [Lifting State Up](https://react.dev/learn/sharing-state-between-components) (A related concept about moving state to a common ancestor).

## 📝 **Mini Task (Production)**

You have been given a `TodoList` component that fetches its own data and also handles all the rendering logic.

1. Identify the "smart" parts (fetching data, dispatching actions) and the "dumb" parts (rendering the list, the loading state, etc.).
2. Refactor this single component into two: `TodoListContainer.tsx` and `TodoListView.tsx`.
3. The container should handle all the Redux logic.
4. The view should receive props like `todos`, `status`, and `onToggleTodo`, and be responsible only for the JSX.
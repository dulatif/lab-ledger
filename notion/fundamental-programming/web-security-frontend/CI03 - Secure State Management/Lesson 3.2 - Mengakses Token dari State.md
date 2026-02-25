💡 CI03.3.2: The Vault Door: Accessing Tokens from State

**Outline:**

- **The Threat (SEEI):** Components directly accessing the internal structure of the state (e.g., `const token = useSelector(state => state.auth.token)`). This creates tight coupling; if you ever refactor the `auth` slice, you have to find and update every single component that uses it.
- **The Defense (PPP):** The principle of "Decoupled Access via Selectors." Creating simple, reusable functions called selectors that abstract away the shape of the state. Components use selectors to ask _what_ they need, not _where_ to find it.
- **Your Mission (Production):** Write selector functions to retrieve the auth token and the current authentication status from the `auth` slice.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **brittle architecture**. When a component's code is directly tied to the shape of your Redux store, you've created a maintenance nightmare.

```ts
// VULNERABLE (TIGHTLY COUPLED) COMPONENT
import { useSelector } from 'react-redux';

function UserAvatar() {
  // This component knows the exact path: `auth`, then `user`, then `avatarUrl`
  const avatarUrl = useSelector(state => state.auth.user.avatarUrl);

  if (!avatarUrl) return null;

  return <img src={avatarUrl} alt="User Avatar" />;
}

```

What happens if you decide to rename the `auth` slice to `session`? Or nest the `user` object under a `profile` key? You would have to find every component that uses `state.auth.user` and manually update it. This is error-prone and scales poorly.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"Components should not know the shape of the state."** A selector is a function that takes the entire state object as an argument and returns a specific piece of it. This creates a layer of abstraction. Your components call the selector, and the selector is the only thing that needs to know the internal structure of the store.

**Secure Code Implementation (Creating and Using Selectors):**

It's a best practice to co-locate selectors with the slice they relate to.

```ts
// SECURE CODE (in authSlice.js)

// ... (createSlice definition from the previous lesson)

// --- SELECTORS ---
// A "base" selector to get the whole auth slice
const selectAuth = state => state.auth;

// A selector that returns only the token
export const selectAuthToken = state => selectAuth(state).token;

// A selector that returns the current user object
export const selectCurrentUser = state => selectAuth(state).user;

// A selector that computes derived data, like authentication status
export const selectIsAuthenticated = state => !!selectAuth(state).token;

// A selector for the loading status
export const selectAuthStatus = state => selectAuth(state).status;

```

Now, our component becomes much cleaner and more resilient to change.

```ts
// SECURE (DECOUPLED) COMPONENT
import { useSelector } from 'react-redux';
// We import the selector, not the knowledge of the state's shape
import { selectCurrentUser } from './authSlice';

function UserAvatar() {
  // The component just asks for the current user.
  const user = useSelector(selectCurrentUser);

  // It doesn't know or care where the user data lives in the store.
  if (!user?.avatarUrl) return null;

  return <img src={user.avatarUrl} alt="User Avatar" />;
}

```

If you now refactor your `authSlice`, you only need to update the selector functions in `authSlice.js`. None of your components will break.

### 🤔 Reflective Questions

1. Selectors can also be "memoized" using libraries like `reselect`. A memoized selector will only re-run its computation if its input has changed. Why is this important for performance, especially for selectors that do complex calculations (e.g., filtering a large list)?
2. How can selectors improve the security of your components? (Hint: Think about what happens if a selector only returns the `user.name` and not the entire user object).

### 📚 Further Reading

- **Redux Style Guide:** [Use Selectors for Reading Store State](https://www.google.com/search?q=https://redux.js.org/style-guide/style-guide%23use-selectors-for-reading-store-state)
- **Reselect:** [GitHub Repository](https://github.com/reduxjs/reselect) - The standard library for creating memoized selectors.

### 📝 Mini Task (Production)

You are building a "Login" button component. This button should be disabled if the authentication status is currently `'loading'`.

Your task: Using the selectors we defined above, write the `useSelector` hook call inside the component that would get the necessary information to determine if the button should be disabled.
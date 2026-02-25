💡 CI03.4.4: The Information Firewall: Designing Secure Selectors

**Outline:**

- **The Threat (SEEI):** A "leaky selector" that returns a large, sensitive object from the state to a component that only needs one or two simple fields. This exposes the entire object to the component and its potential vulnerabilities.
- **The Defense (PPP):** The principle of "Granular Data Access." Designing selectors to be highly specific, returning only the primitive values (strings, numbers) or minimal objects that a component requires to render.
- **Your Mission (Production):** Refactor a selector that returns a full user object into two separate, more secure selectors.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **over-exposure at the component level**. Even if you've done a great job minimizing the data in your Redux store, you can still create risk by how you expose that data to your components. A selector acts as a pipe from your state to your component; if the pipe is too wide, it can leak.

**How it Works (The Attack Vector):**

Your Redux state is secure and minimal.

```ts
// SECURE STATE
const state = {
  auth: { user: { name: "Jane Doe", email: "jane.doe@example.com" } }
};

```

You write a generic selector that returns the whole user object.

```ts
// LEAKY SELECTOR
export const selectCurrentUser = state => state.auth.user;

```

Now, you build a simple `Greeting` component for the header.

```ts
// VULNERABLE COMPONENT
import { useSelector } from 'react-redux';
import { selectCurrentUser } from './authSlice';

function Greeting() {
  // This component now has access to the user's name AND email.
  const user = useSelector(selectCurrentUser);

  // It only needs the name.
  return <p>Hello, {user.name}!</p>;
}

```

The problem? The `Greeting` component now holds a reference to the user's `email`, even though it never uses it. If this component (or a child component it renders) has a bug or is compromised by an XSS attack that can read component props/state, the email address could be leaked. The component was given a privilege (access to the email) that it did not need.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"A selector should provide the minimum data necessary for its consumer."** Don't return entire objects when a single field will do. Create specific, granular selectors that act as a firewall between your state and your UI.

**Secure Code Implementation (Granular Selectors):**

Instead of one generic selector, we create several highly specific ones.

```ts
// SECURE (GRANULAR) SELECTORS (in authSlice.js)

// Base selector (private to this file if possible)
const selectAuth = state => state.auth;

// Returns only the user's name, or null if no user
export const selectCurrentUserName = state => selectAuth(state).user?.name ?? null;

// Returns only the user's email, or null
export const selectCurrentUserEmail = state => selectAuth(state).user?.email ?? null;

// Returns a boolean, not the whole object
export const selectIsAuthenticated = state => !!selectAuth(state).user;

```

Now, we refactor the `Greeting` component to use the correct, minimal selector.

```ts
// SECURE COMPONENT
import { useSelector } from 'react-redux';
// We import the specific selector we need
import { selectCurrentUserName } from './authSlice';

function Greeting() {
  // This component now ONLY receives the name string.
  // It has no knowledge of the email address.
  const userName = useSelector(selectCurrentUserName);

  if (!userName) return null;

  return <p>Hello, {userName}!</p>;
}

```

We have successfully enforced the Principle of Least Privilege. The component's attack surface has been reduced.

### 🤔 Reflective Questions

1. Creating many small selectors can feel like more boilerplate. What are the long-term benefits for maintainability and refactoring when your codebase has hundreds of components?
2. Memoized selectors (from `reselect`) are often composed. For example, `selectCurrentUserName` might be created from `selectCurrentUser`. How does this pattern help both performance and code reuse?

### 📚 Further Reading

- **Redux Style Guide:** [Use Selectors for Reading Store State](https://www.google.com/search?q=https://redux.js.org/style-guide/style-guide%23use-selectors-for-reading-store-state) (Re-read this with a focus on granularity).

### 📝 Mini Task (Production)

Your state contains a `settings` slice with the following shape: `{ theme: 'dark', language: 'en', notifications: { enabled: true, sound: 'default' } }`

You are building a `ThemeToggleButton` component that only needs to know the current `theme`.

Your task: Write a granular selector named `selectCurrentTheme` that extracts only the `theme` string from the state.
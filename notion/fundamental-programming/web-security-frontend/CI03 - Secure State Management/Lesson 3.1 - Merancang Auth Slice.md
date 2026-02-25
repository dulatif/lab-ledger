💡 CI03.3.1: The Control Room: Designing the Auth Slice

**Outline:**

- **The Threat (SEEI):** A disorganized authentication state, where tokens, user data, loading flags, and error messages are scattered or handled inconsistently. This leads to bugs, race conditions, and makes the auth flow difficult to secure and maintain.
- **The Defense (PPP):** The principle of a "Single Source of Truth for Auth." Using Redux Toolkit's `createSlice` to create a dedicated, well-structured slice that manages every piece of authentication-related state in one predictable place.
- **Your Mission (Production):** Create an `authSlice` using Redux Toolkit that defines the initial state and reducers for handling login success, failure, and logout.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **state fragmentation**. In a complex app, authentication isn't just a single `isAuthenticated` boolean. It involves:

- The user's token (e.g., JWT).
- The user's profile data.
- A loading status (e.g., `'idle' | 'loading'`).
- Potential error messages.

If these pieces of state are managed separately (e.g., in different slices, or worse, in multiple `useState` hooks across the app), your logic becomes a tangled mess. It's easy to create impossible states, like having a user token but `isAuthenticated` being false, or showing a logged-in UI while a token refresh is actually failing in the background.

**How it Works (The Attack Vector):**

This is a bug-driven vulnerability, not a direct attack.

```ts
// VULNERABLE (FRAGMENTED) STATE
// In one component...
const [token, setToken] = useState(null);
// In another component...
const [currentUser, setCurrentUser] = useState(null);
// In the Redux store...
const authState = { isLoading: false, error: null };

```

A bug in the login flow might set the `token` but fail to set the `currentUser`. A different part of the UI, only checking for `currentUser`, might think the user is logged out, while another part, only checking for `token`, might try to make authenticated API calls, leading to unpredictable behavior and potential information leaks in error messages.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"Consolidate all related state into a single, cohesive slice."** Redux Toolkit's `createSlice` is the perfect tool for this. It allows you to define the initial state, the reducers that can change that state, and the actions that trigger those reducers, all in one object. This makes your authentication logic predictable, testable, and easier to secure.

**Secure Code Implementation (Using `createSlice`):**

First, install Redux Toolkit: `npm install @reduxjs/toolkit`

Now, let's design a robust `authSlice`.

```ts
// SECURE CODE (authSlice.js)
import { createSlice } from '@reduxjs/toolkit';

const initialState = {
  user: null, // Holds user data object
  token: null, // Holds the JWT
  status: 'idle', // 'idle' | 'loading' | 'succeeded' | 'failed'
  error: null,
};

const authSlice = createSlice({
  name: 'auth',
  initialState,
  // Reducers define how the state can be changed
  reducers: {
    loginStart(state) {
      state.status = 'loading';
      state.error = null;
    },
    loginSuccess(state, action) {
      // The action payload will contain the user and token
      state.status = 'succeeded';
      state.user = action.payload.user;
      state.token = action.payload.token;
    },
    loginFailed(state, action) {
      state.status = 'failed';
      state.error = action.payload;
      state.user = null;
      state.token = null;
    },
    logout(state) {
      // Reset the state to its initial values
      state.user = null;
      state.token = null;
      state.status = 'idle';
      state.error = null;
    },
  },
});

// Export the actions so components can dispatch them
export const { loginStart, loginSuccess, loginFailed, logout } = authSlice.actions;

// Export the reducer to be included in the main store
export default authSlice.reducer;

```

This slice provides a complete, self-contained state machine for your authentication flow.

### 🤔 Reflective Questions

1. In our `initialState`, `user` and `token` are `null`. How does this "default to secure" initial state help prevent bugs related to premature access to protected resources?
2. We've only defined synchronous reducers. How would you use Redux Toolkit's `createAsyncThunk` to handle the asynchronous logic of making a login API call and dispatching `loginStart`, `loginSuccess`, or `loginFailed`?

### 📚 Further Reading

- **Redux Toolkit:** [`createSlice` API Reference](https://redux-toolkit.js.org/api/createSlice)

### 📝 Mini Task (Production)

Your `authSlice` needs to handle token refreshes. A successful refresh provides a new token, but the user object stays the same.

Your task: Add a new reducer to the `authSlice` called `tokenRefreshed` that takes an action with the new token in its payload and updates _only_ the `token` field in the state, leaving the `user`, `status`, and `error` fields untouched.
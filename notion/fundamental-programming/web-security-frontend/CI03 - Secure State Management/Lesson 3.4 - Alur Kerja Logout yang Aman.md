💡 CI03.3.4: The Clean Exit: Secure Logout Workflows

**Outline:**

- **The Threat (SEEI):** An incomplete logout process. Simply clearing client-side state is not enough. If the session token (JWT) is not invalidated on the server, a stolen token could still be used to access the application until it expires.
- **The Defense (PPP):** The principle of "Full Session Termination." A secure logout must be a coordinated effort between the client and the server. The client must clear all local state, and the server must invalidate the token.
- **Your Mission (Production):** Design a Redux Thunk for logout that performs all the necessary client-side and server-side cleanup actions.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is a **dangling session**. A developer implements a logout button that only clears the local Redux state.

```ts
// VULNERABLE (CLIENT-ONLY) LOGOUT
const handleLogout = () => {
  // This only affects the current browser tab.
  dispatch(logout());
};

```

From the user's perspective, they appear to be logged out. The UI returns to the login screen. However, the JWT that was stored in their state is still perfectly valid. If an attacker had previously stolen that token (e.g., through XSS or a data leak), they can continue to use it to make authenticated API requests on the user's behalf until the token naturally expires, which could be hours or days later.

**How it Works (The Attack Vector):**

1. A user is logged in. Their valid JWT is `token_abc`.
2. An attacker steals `token_abc`.
3. The user clicks the "Logout" button. The client dispatches the `logout` action, clearing the Redux state. The UI shows they are logged out.
4. The attacker, from their own machine, makes an API call using the stolen token: `curl -H "Authorization: Bearer token_abc" <https://your-app.com/api/profile`>.
5. Because the server was never told the token is invalid, it accepts the request, and the attacker successfully fetches the user's profile data.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"Logout is a two-way street."** A secure logout requires three steps:

1. **Server Invalidation:** Make an API call to a `/logout` endpoint. The server should then add the token's unique ID (the `jti` claim in a JWT) to a "deny list" so it can no longer be used.
2. **Client State Reset:** Dispatch the `logout` action in your `authSlice` to clear the token, user info, and reset the state to its initial, unauthenticated form.
3. **Persisted State Purge:** If you are using `redux-persist`, you must also purge the persisted state from storage to ensure the old, encrypted data is removed.

**Secure Code Implementation (Redux Thunk):**

A Redux Thunk is the perfect tool to orchestrate these asynchronous steps.

```ts
// SECURE CODE (in a new file, e.g., authThunks.js)
import { createAsyncThunk } from '@reduxjs/toolkit';
import apiClient from '../api';
import { logout as logoutAction } from './authSlice';
import { persistor } from '../store'; // Import your persistor object

export const performLogout = createAsyncThunk(
  'auth/performLogout',
  async (_, { dispatch }) => {
    try {
      // Step 1: Tell the server to invalidate the token.
      // We don't care about the response, just that it completes.
      // The interceptor will add the current token to this request.
      await apiClient.post('/logout');
    } catch (error) {
      // Even if the server call fails, we must proceed with client-side logout.
      console.error("Server logout failed, proceeding with client cleanup:", error);
    } finally {
      // Step 2: Clear the Redux state.
      dispatch(logoutAction());

      // Step 3: Purge the persisted state from localStorage.
      // This ensures no encrypted data is left behind.
      await persistor.purge();
    }
  }
);

```

Your logout button component would now simply `dispatch(performLogout())`.

### 🤔 Reflective Questions

1. In the `catch` block, we proceed with the client-side logout even if the server call fails. Why is this the correct security posture?
2. Server-side token deny lists add complexity and a performance cost (every request requires a check against the list). What is an alternative server-side strategy using short-lived access tokens and long-lived refresh tokens? How does logging out work in that model?

### 📚 Further Reading

- **OWASP:** [JSON Web Token (JWT) Cheat Sheet for Java - Invalidation](https://www.google.com/search?q=https://cheatsheetseries.owasp.org/cheatsheets/JSON_Web_Token_for_Java_Cheat_Sheet.html%23jwt-invalidation) - Discusses server-side strategies for invalidating tokens.

### 📝 Mini Task (Production)

After a successful logout, you want to redirect the user back to the homepage (`/`).

Your task: Where in the `performLogout` thunk or the component that calls it would you trigger this redirect? Explain the pros and cons of doing it inside the thunk versus in the component.
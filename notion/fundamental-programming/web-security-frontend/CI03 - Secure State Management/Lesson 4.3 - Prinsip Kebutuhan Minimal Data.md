💡 CI03.4.3: Less is More: The Principle of Minimal Data

**Outline:**

- **The Threat (SEEI):** Over-fetching and over-storing data. API endpoints often return large, nested objects, and developers store the entire object in the state, even when the UI only needs a few fields. This needlessly increases the client-side attack surface.
- **The Defense (PPP):** The principle of "Just Enough, Just in Time." On the backend, create dedicated endpoints that return only the data a specific view needs. On the frontend, only store the minimal data required in your state.
- **Your Mission (Production):** Refactor a Redux action and state slice to only store a subset of a large API response object.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **excessive data exposure**. It's a violation of the "Principle of Least Privilege" applied to data. Your frontend code should only have access to the data it absolutely needs to function. Storing unnecessary sensitive data in the client's memory is a security risk.

**How it Works (The Attack Vector):**

An API endpoint `/api/user/me` returns the full user object from the database.

```ts
// Full API Response
{
  "id": 123,
  "name": "Jane Doe",
  "email": "jane.doe@example.com",
  "avatarUrl": "...",
  "lastLoginIp": "192.168.1.1", // Sensitive
  "accountBalance": 500, // Sensitive
  "isAdmin": false,
  "preferences": { ... }
}

```

A developer needs to display the user's name and avatar in the site header. They write a thunk that fetches this data and puts the _entire object_ into the Redux store.

```ts
// VULNERABLE (OVER-STORING) STATE
const state = {
  auth: {
    token: '...',
    // The entire sensitive object is now in memory
    user: {
      id: 123,
      name: "Jane Doe",
      email: "jane.doe@example.com",
      avatarUrl: "...",
      lastLoginIp: "192.168.1.1",
      accountBalance: 500,
      // ... and so on
    }
  }
};

```

Now, the `lastLoginIp` and `accountBalance` are sitting in the browser's memory for the entire session. If an attacker finds an XSS vulnerability, they can easily exfiltrate this data. The header component never needed it, but by storing it, we've created an unnecessary risk.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"Shape your data for the view."** Don't just dump raw API responses into your state. Your state should be a curated, minimal representation of what the UI actually needs.

**Secure Code Implementation:**

**1. The Backend Fix (Ideal):** The best solution is to create a new, dedicated API endpoint.

- `/api/user/session-info` -> Returns only `{ name, avatarUrl }`. This endpoint is designed specifically for the header component. It never exposes the sensitive data in the first place.

**2. The Frontend Fix (Pragmatic):** If you can't change the backend, you must transform the data on the client before it enters your store.

```ts
// SECURE REDUX THUNK
import { createAsyncThunk } from '@reduxjs/toolkit';

export const fetchSessionUser = createAsyncThunk(
  'auth/fetchSessionUser',
  async () => {
    const response = await apiClient.get('/api/user/me');
    // This is the crucial transformation step.
    // We create a new, smaller object with only the data we need.
    const sessionUser = {
      name: response.data.name,
      avatarUrl: response.data.avatarUrl,
    };
    return sessionUser; // Only this safe object will go to the reducer.
  }
);

// In your authSlice:
const authSlice = createSlice({
  //...
  extraReducers: (builder) => {
    builder.addCase(fetchSessionUser.fulfilled, (state, action) => {
      // action.payload is now the small, safe sessionUser object
      state.user = action.payload;
    });
  },
});

```

Now, your Redux state will be minimal and safe.

```ts
// SECURE (MINIMAL) STATE
const state = {
  auth: {
    token: '...',
    user: {
      name: "Jane Doe",
      avatarUrl: "...",
    } // No sensitive data!
  }
};

```

### 🤔 Reflective Questions

1. This principle seems to conflict with the idea of a server cache like React Query, which often caches the full response. How can these two ideas be reconciled? (Hint: Think about where the transformation happens).
2. What are the performance benefits of fetching smaller, dedicated data payloads from the backend?

### 📚 Further Reading

- **OWASP:** [API Security Top 10 - Excessive Data Exposure](https://www.google.com/search?q=https://owasp.org/API-Security/editions/2023/en/0x3a-excessive-data-exposure/)

### 📝 Mini Task (Production)

An API endpoint `/api/posts/1` returns a large post object: `{ id, title, content, author, comments, revisionHistory, editorNotes }`.

You are building a component that displays a list of post titles. It only needs the `id` and `title` for each post.

Your task: Write a small JavaScript function called `transformPostForListView` that takes the full post object as an argument and returns a new object containing only the `id` and `title`.
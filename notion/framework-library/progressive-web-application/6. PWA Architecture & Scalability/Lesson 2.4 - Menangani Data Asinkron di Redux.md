# 💡 AT01.2.4: The Connected Brain: Handling Async Data in Redux

**Outline:**

- **State and the Server (Presentation):** The challenge of synchronizing client-side state with a remote server.
- **The Power of Thunks (Practice):** Using Redux Toolkit's `createAsyncThunk` to manage the lifecycle of API requests.
- **Fetching Your First Data (Production):** Implementing a thunk to load data into your Redux store.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - State and the Server**

Our PWA now has a persistent brain, but it can't live in isolation forever. It needs to communicate with the outside world—to fetch new data, submit user changes, and stay in sync with a backend server. Handling these asynchronous operations is a core task of any state management library.

The challenge is managing the entire lifecycle of a request. An API call isn't a single event; it's a process with at least three states:

1. **Pending:** The request has been sent, and we're waiting for a response. We should probably show a loading indicator.
2. **Fulfilled:** We received a successful response with the data we wanted. We need to update our store with this new data and hide the loading indicator.
3. **Rejected:** The request failed (network error, server error). We need to store the error message and show it to the user.

Managing this manually in components with `useEffect` leads to a lot of repeated, complex, and error-prone code. Redux Toolkit provides a clean, centralized, and robust pattern for this: **Async Thunks**.

### **Part 2: Practice - The Power of Thunks**

A "thunk" is simply a function that can contain async logic and can dispatch Redux actions. Redux Toolkit's `createAsyncThunk` function is a powerful abstraction that generates a thunk and automatically handles dispatching the pending, fulfilled, and rejected actions for us.

**Step 1: Create the async thunk** This function will contain our actual API call.

```ts
// features/todos/todosApi.ts
import { createAsyncThunk } from '@reduxjs/toolkit';
import axios from 'axios';

// The first argument is a string that becomes the prefix for the generated action types.
export const fetchTodos = createAsyncThunk('todos/fetchTodos', async () => {
  const response = await axios.get('<https://jsonplaceholder.typicode.com/todos?_limit=5>');
  return response.data; // This value becomes the payload of the 'fulfilled' action
});
```

**Step 2: Handle the actions in your slice** We use the `extraReducers` builder to listen for the actions dispatched by our thunk.

```ts
// features/todos/todosSlice.ts
import { createSlice } from '@reduxjs/toolkit';
import { fetchTodos } from './todosApi';

const todosSlice = createSlice({
  name: 'todos',
  initialState: {
    items: [],
    status: 'idle', // 'idle' | 'loading' | 'succeeded' | 'failed'
    error: null,
  },
  reducers: {
    // ... your standard synchronous reducers go here
  },
  // We handle the async actions here
  extraReducers: (builder) => {
    builder
      .addCase(fetchTodos.pending, (state) => {
        state.status = 'loading';
      })
      .addCase(fetchTodos.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.items = action.payload; // Set the items from the API response
      })
      .addCase(fetchTodos.rejected, (state, action) => {
        state.status = 'failed';
        state.error = action.error.message;
      });
  },
});

export default todosSlice.reducer;
```

Now, a component can simply dispatch `fetchTodos()`, and the slice will automatically handle all the loading and error states in a predictable way. The component's only job is to select the data and status from the store and render the appropriate UI.

## 🧠 **Real-World Case Study: "The Tangled `useEffect`"**

- **Before:** A React component for displaying a user's profile had a complex `useEffect` hook. It contained multiple `useState` calls for `data`, `isLoading`, and `error`. It had logic to handle the fetch call, set loading states, catch errors, and an abort controller for cleanup. The logic was over 30 lines long and was tightly coupled to the component.
- **The Refactor:** The team moved the entire async logic into a `fetchUserProfile` thunk using `createAsyncThunk`. They added the `extraReducers` to their `userSlice`.
- **After:** The component became incredibly simple. The giant `useEffect` was replaced with a single line: `useEffect(() => { dispatch(fetchUserProfile(userId)); }, [dispatch, userId]);`. The component now just used `useSelector` to get the `user`, `status`, and `error` from the global store. The business logic was completely decoupled from the view, making both easier to test and maintain.

## 🤔 **Reflective Questions**

1. What is the main benefit of using the `extraReducers` builder syntax instead of a simple object?
2. How does the `createAsyncThunk` pattern fit into an offline-first PWA? How could you combine it with your persisted state to create an optimistic UI? (e.g., show persisted data first, then fetch in the background).
3. RTK has another, more advanced tool for data fetching and caching called RTK Query. What are some of the problems it solves on top of `createAsyncThunk`?

## 📚 **Further Reading**

- **Redux Toolkit:** [Async Logic and Data Fetching](https://www.google.com/search?q=https://redux-toolkit.js.org/tutorials/essentials/part-5-async-logic) (The official tutorial on thunks).
- **Redux Toolkit:** [RTK Query](https://redux-toolkit.js.org/rtk-query/overview) (For when you're ready to level up your data fetching).

## 📝 **Mini Task (Production)**

Connect your application's brain to the network.

1. Add a `todosSlice` to your Redux store, including the `status` and `error` fields in its initial state.
2. Create an async thunk `fetchTodos` that fetches a few todos from the public `https://jsonplaceholder.typicode.com/todos` API.
3. Implement the `extraReducers` in your `todosSlice` to handle the pending, fulfilled, and rejected states from your thunk.
4. In a component, dispatch your `fetchTodos` thunk inside a `useEffect`.
5. Render a loading message when `status` is `'loading'`, an error message when `status` is `'failed'`, and the list of todo titles when `status` is `'succeeded'`.
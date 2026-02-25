💡 CI03.3.3: The Automatic Keycard: Auto-Injecting Tokens in APIs

**Outline:**

- **The Threat (SEEI):** Manually adding the authentication token to the header of every single API request. This is highly repetitive, easy to forget, and scatters your authentication logic across dozens of files.
- **The Defense (PPP):** The principle of "Intercept and Inject." Using a middleware for your API client (like an Axios interceptor) that automatically reads the token from the Redux store and adds the `Authorization` header to every outgoing request.
- **Your Mission (Production):** Write an Axios request interceptor that connects to the Redux store to automatically add the `Bearer` token to API calls.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **inconsistent implementation**. When you require developers to remember a security-critical step every time they perform a common action, they will eventually forget.

```ts
// VULNERABLE (MANUAL) API CALL
import axios from 'axios';

// This is inside a component or thunk
async function updateUserProfile(dispatch, getState) {
  try {
    const token = getState().auth.token; // 1. Remember to get the token
    const response = await axios.put('/api/profile', data, {
      headers: {
        'Authorization': `Bearer ${token}` // 2. Remember to format the header correctly
      }
    });
    // ...
  } catch (error) {
    // ...
  }
}

```

This pattern has to be repeated for every authenticated endpoint. If a developer forgets step 1 or 2 on a new feature, that API call will fail with a `401 Unauthorized` error, leading to bugs. It's also verbose and mixes a cross-cutting concern (authentication) with feature-specific logic.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"Centralize your cross-cutting concerns."** Authentication headers are a perfect example. Your API client library (like Axios) should be configured _once_ to handle this automatically for all relevant requests. An "interceptor" or "middleware" is a function that can inspect and modify a request before it's sent.

**Secure Code Implementation (Axios Interceptor):**

First, install Axios: `npm install axios`

Next, create a dedicated API client instance that is "aware" of your Redux store.

```ts
// SECURE CODE (api.js)
import axios from 'axios';
import { store } from './store'; // Import your Redux store instance
import { selectAuthToken } from './features/auth/authSlice'; // Import the selector

// 1. Create a new Axios instance
const apiClient = axios.create({
  baseURL: '/api',
});

// 2. Add a request interceptor
apiClient.interceptors.request.use(
  (config) => {
    // Get the token from the Redux store
    const token = selectAuthToken(store.getState());

    // If a token exists, add the Authorization header
    if (token) {
      config.headers['Authorization'] = `Bearer ${token}`;
    }

    // Must return the config object, or the request will be blocked
    return config;
  },
  (error) => {
    // Handle request errors
    return Promise.reject(error);
  }
);

export default apiClient;

```

Now, your components and thunks can use this client instance without ever thinking about tokens.

```ts
// CLEAN, DECOUPLED API CALL
import apiClient from './api';

async function updateUserProfile(data) {
  // No need to get state or manually set headers.
  // The interceptor handles it automatically.
  const response = await apiClient.put('/profile', data);
  // ...
}

```

This is cleaner, safer, and far more maintainable.

### 🤔 Reflective Questions

1. Axios also supports "response" interceptors, which run after a response is received. How could you use a response interceptor to automatically handle `401 Unauthorized` errors (e.g., by dispatching a logout action)?
2. This pattern requires your API client module to import the Redux store instance, which can sometimes create circular dependency issues. What are some other ways to provide the token to the interceptor (e.g., passing `getState` as an argument)?

### 📚 Further Reading

- **Axios:** [Interceptors](https://axios-http.com/docs/interceptors) - Official documentation on how to use interceptors.

### 📝 Mini Task (Production)

Your API requires another custom header, `X-Client-ID`, to be sent with every request, in addition to the `Authorization` header. The value for this ID is a static string, `'my-react-app'`.

Your task: Modify the Axios request interceptor code to add this second header to every outgoing request.
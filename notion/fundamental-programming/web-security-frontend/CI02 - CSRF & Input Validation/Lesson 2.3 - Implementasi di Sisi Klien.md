💡 CI02.2.3: The Client as Messenger: Client-Side Implementation

**Outline:**

- **The Threat (SEEI):** A secure backend that rejects all requests because the client-side application (the SPA) isn't correctly fetching and sending the required CSRF token.
- **The Defense (PPP):** Implementing a robust client-side pattern to fetch the CSRF token once and automatically include it in all subsequent state-changing API calls.
- **Your Mission (Production):** Modify a `fetch` call to correctly include a CSRF token.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

This isn't a security vulnerability in the traditional sense, but a **functional failure caused by a security requirement**. The server is correctly configured to reject any state-changing request that lacks a valid CSRF token. The frontend developer, unaware of this requirement or how to implement it, builds a form that sends a `POST` request without the token. The result: every form submission fails with a `403 Forbidden` error. The application is "secure" but completely broken.

**How it Works (The Attack Vector):**

A React developer writes a standard form submission handler.

```ts
// VULNERABLE (NON-FUNCTIONAL) CODE
const handleChangePassword = async (newPassword) => {
  try {
    const response = await fetch('/api/settings/change-password', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ newPassword }),
    });

    if (response.ok) {
      // This success case is never reached!
      alert('Password changed!');
    } else {
      // The code always ends up here.
      alert('Error: ' + response.statusText); // "Forbidden"
    }
  } catch (error) {
    console.error('Failed to change password', error);
  }
};

```

The developer didn't include the `X-CSRF-Token` header that the server's `csrfProtection` middleware is expecting. The security feature is working, but the client isn't complying with the protocol.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The client-side principle is: **"Fetch Once, Send Always."** Your Single-Page Application should fetch the CSRF token from the server when it first loads and then store it. Every subsequent state-changing request must be configured to read this stored token and include it in its headers. The best way to achieve this is by creating a centralized API wrapper or custom hook.

**Secure Code Implementation (React Example):**

**1. Storing the Token:** We can fetch and store the token in our main `App` component.

```ts
// SECURE CODE - App.js
import { useState, useEffect } from 'react';

// This will be our global API client
export const apiClient = {
  _csrfToken: null,
  // ... methods will go here
};

function App() {
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    // Fetch the token when the app loads
    const getToken = async () => {
      const response = await fetch('/api/csrf-token');
      const data = await response.json();
      apiClient._csrfToken = data.csrf_token;
      setIsLoading(false);
    };
    getToken();
  }, []);

  if (isLoading) return <div>Loading...</div>;

  // ... rest of the app
}

```

**2. Creating an API Wrapper:** Now, we create a wrapper around `fetch` that automatically adds the token.

```ts
// SECURE CODE - apiClient object
export const apiClient = {
  _csrfToken: null,

  async post(url, body) {
    return this._request('POST', url, body);
  },

  async _request(method, url, body) {
    const headers = {
      'Content-Type': 'application/json',
    };
    // Add the token to state-changing methods
    if (this._csrfToken && ['POST', 'PUT', 'DELETE'].includes(method)) {
      headers['X-CSRF-Token'] = this._csrfToken;
    }

    const response = await fetch(url, {
      method,
      headers,
      body: JSON.stringify(body),
    });

    if (!response.ok) {
      throw new Error(`API Error: ${response.statusText}`);
    }
    return response.json();
  }
};

```

**3. Using the Wrapper:** Our component code is now much cleaner and correctly sends the token.

```ts
// SECURE CODE - Component
import { apiClient } from './apiClient';

const handleChangePassword = async (newPassword) => {
  try {
    // The wrapper handles the token automatically!
    await apiClient.post('/api/settings/change-password', { newPassword });
    alert('Password changed!');
  } catch (error) {
    alert('Error: ' + error.message);
  }
};

```

### 🤔 Reflective Questions

1. Why is it better to create a centralized `apiClient` or custom hook for this logic instead of adding the CSRF token header inside every single component that makes a POST request?
2. In our `App.js` example, we stored the token in a simple JavaScript object. What are other potential places you could store the token on the client side, and what are their pros and cons (e.g., component state, context, local storage)?

### 📚 Further Reading

- **Axios:** [Interceptors](https://axios-http.com/docs/interceptors) - Many developers use the library `axios`. Its "interceptor" feature is a perfect, production-grade way to implement this "add header to every request" pattern.

### 📝 Mini Task (Production)

You are not using a fancy API wrapper, just the plain `fetch` API. You have already fetched a CSRF token and stored it in a variable called `csrfToken`.

Your task: Rewrite the `headers` object in the `fetch` call below to correctly include the token in an `X-CSRF-Token` header.

```ts
const csrfToken = 'n9v8b7a6s5d4f3g2h1j0k'; // This is already available

const createPost = async (title) => {
  await fetch('/api/posts', {
    method: 'POST',
    // --- START EDITING HERE ---
    headers: {
      'Content-Type': 'application/json',
    },
    // --- END EDITING HERE ---
    body: JSON.stringify({ title }),
  });
};

```
💡 AA01-2.3: Handling Expired Tokens with Interceptors

**Outline:**

- **The Threat (SEEI):** Understanding the terrible code architecture and UX that comes from manually handling token expiration for every API call.
- **The Defense (PPP):** Introducing API client interceptors as a centralized, elegant way to handle `401 Unauthorized` responses.
- **Your Mission (Production):** Time to write your first Axios interceptor.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **Code Duplication and Inconsistent Error Handling.** Without a centralized mechanism, every developer on your team who needs to fetch protected data has to write their own logic to handle a potential `401 Unauthorized` error. This is a massive violation of the DRY (Don't Repeat Yourself) principle.
    
- **How it Works (The Attack Vector / Bad Pattern):** Imagine you have multiple components that fetch data.
    
    ```ts
    // BAD PATTERN: REPEATED LOGIC
    
    // In ProfilePage.tsx
    useEffect(() => {
      fetch('/api/user')
        .then(res => {
          if (res.status === 401) {
            // Oh no, it expired. What do I do?
            // Try to refresh? Redirect to login?
            // Let's just redirect...
            window.location = '/login';
          }
          return res.json();
        });
    }, []);
    
    // In OrdersPage.tsx
    useEffect(() => {
      fetch('/api/orders')
        .then(res => {
          if (res.status === 401) {
            // This developer forgot to handle the 401.
            // The app will probably crash trying to call .json() on the error response.
            // A silent bug is born.
          }
          return res.json();
        });
    }, []);
    
    ```
    
    This leads to inconsistent behavior (some pages redirect, others crash), bugs, and a maintenance nightmare. There is no single source of truth for handling authentication errors.
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Centralize your cross-cutting concerns.** Authentication logic is a "cross-cutting concern"—it applies to nearly every API request your application makes. A client-side API interceptor allows you to define this logic _once_ and have it apply automatically to all requests. Libraries like Axios and the `fetch` API have middleware capabilities that make this possible.
    
- **Secure Code Implementation:** Let's create a dedicated API client instance using Axios and attach a response interceptor to it.
    
    ```ts
    // api.ts - A dedicated module for our Axios instance
    
    import axios from 'axios';
    
    // Create an instance of Axios
    const apiClient = axios.create({
      baseURL: '<https://yourapi.com/api>',
      withCredentials: true, // This is crucial for sending cookies
    });
    
    // Add a response interceptor
    apiClient.interceptors.response.use(
      // The first function is for successful responses (2xx status codes)
      // We don't need to do anything here, so we just return the response.
      (response) => {
        return response;
      },
      // The second function is for error responses
      (error) => {
        // Check if the error is a 401 Unauthorized
        if (error.response && error.response.status === 401) {
          // THIS IS WHERE THE MAGIC HAPPENS!
          console.log('Caught a 401 Error. Time to refresh the token.');
          // In the next lesson, we will implement the refresh logic here.
          // For now, we can redirect to login as a simple fallback.
          window.location = '/login';
        }
    
        // It's important to reject the promise for other errors
        // so that component-level .catch() blocks can handle them.
        return Promise.reject(error);
      }
    );
    
    export default apiClient;
    
    ```
    
    Now, any component in your app can use this `apiClient` and the 401 handling is automatic and consistent.
    
    ```ts
    // In any component...
    import apiClient from './api';
    
    useEffect(() => {
      // No need for manual 401 checks here!
      apiClient.get('/user')
        .then(response => setUser(response.data))
        .catch(error => console.error("This will only catch non-401 errors.", error));
    }, []);
    
    ```
    

### 🧠 Real-World Case Study: "The Personal Assistant vs. The Centralized Receptionist"

- **The Goal:** Handle incoming phone calls to a busy office where callers might have invalid credentials.
- **Vulnerable Approach (Manual Handling):** Every employee (`component`) has to answer their own phone. When they get a call from someone who isn't authorized (`401 error`), each employee has to decide what to do. One might hang up, another might try to transfer them, a third might write down a message. The result is chaos and inconsistency.
- **Secure Approach (Interceptor):** The company hires a professional receptionist (`interceptor`). All calls are routed through this receptionist first. The receptionist has a single, clear set of instructions: "If a call comes in from an unauthorized number, do not pass it to the employee. Instead, execute the 'security protocol' (our refresh logic)." This frees up every employee to focus on their work, confident that all incoming calls are pre-screened and handled consistently.

### 🤔 Reflective Questions

1. The interceptor code uses `Promise.reject(error)` for non-401 errors. Why is this important for the components that are making the API calls?
2. What is the purpose of `withCredentials: true` in the Axios configuration? What would happen if you forgot to include it?
3. Besides error handling, what are some other useful applications for request interceptors (which run _before_ a request is sent)?

### 📚 Further Reading

- **Axios Docs:** [Interceptors](https://axios-http.com/docs/interceptors)
- **MDN Web Docs:** [A using fetch (see section on intercepting requests and responses)](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- **DigitalOcean Tutorial:** [How To Handle Errors in an Axios Interceptor](https://www.google.com/search?q=https://www.digitalocean.com/community/tutorials/how-to-handle-errors-in-an-axios-interceptor)

### 📝 Mini-Task (Production)

You are setting up the API client for your new project.

Your task: Complete the `api.ts` file below. Create an Axios instance and attach a response interceptor. The interceptor should log a specific message to the console for `401 Unauthorized` errors and another generic message for any other type of error (like `500 Internal Server Error` or `404 Not Found`). Ensure the promise is correctly passed along in all cases.

```ts
import axios from 'axios';

const apiClient = axios.create({
  baseURL: '/api',
  withCredentials: true,
});

apiClient.interceptors.response.use(
  (response) => {
    // For successful responses, just return the response.
    return response;
  },
  (error) => {
    // YOUR IMPLEMENTATION GOES HERE
    // Check for the error response status and log the appropriate message.
    // Remember to return a rejected promise so calling code can handle it.
  }
);

export default apiClient;

```
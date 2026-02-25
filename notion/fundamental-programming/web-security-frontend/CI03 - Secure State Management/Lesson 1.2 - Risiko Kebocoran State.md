💡 CI03.1.2: Spilling Secrets: The Risks of State Leakage

**Outline:**

- **The Threat (SEEI):** Sensitive data from your state store being accidentally exposed to unauthorized parties through common development practices like logging or debugging.
- **The Defense (PPP):** The principle of "Controlled Visibility." Implementing strict controls to prevent the state from being logged in production or exposed on global objects.
- **Your Mission (Production):** Refactor a logging call to prevent it from leaking sensitive PII.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **unintentional exposure**. Developers often use `console.log` and the `window` object as powerful debugging tools. However, if these tools are used carelessly and the code is deployed to production, they become security holes. A third party, such as a browser extension, a Cross-Site Scripting (XSS) payload, or even just another developer on a shared machine, could potentially access this leaked data.

**How it Works (The Attack Vector):**

**Vector 1: Production Logging** A developer leaves a `console.log` statement in the code.

```ts
// VULNERABLE CODE
function userProfileReducer(state, action) {
  // ...
  if (action.type === 'USER_FETCH_SUCCESS') {
    const newState = { ...state, ...action.payload };
    // This logs the user object, including email, token, etc.,
    // to the browser console in production.
    console.log('User data updated:', newState);
    return newState;
  }
  // ...
}

```

An attacker who gains temporary physical access to a logged-in user's computer can simply open the developer console and see the sensitive data. Worse, if an XSS vulnerability exists, the attacker's script could potentially override `console.log` to send the logged data to their own server.

**Vector 2: Exposing State on `window`** For easy debugging, a developer attaches the store's state to the global `window` object.

```ts
// VULNERABLE CODE (in main app setup)
if (process.env.NODE_ENV === 'development') {
  // This is meant for debugging, but what if the check is forgotten?
  window.appState = store.getState();
}

```

If this code makes it to production, any script running on the page (including malicious scripts from an XSS attack) can read the entire application state by simply accessing `window.appState`.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"The browser console and global objects are public spaces in production."** Treat them as such. Never log sensitive data or attach it to `window` in your production builds. Use environment variables to strictly control what debugging information is available.

**Secure Code Implementation:**

**1. Safe Logging:** Instead of logging entire objects, log only what is necessary, or use a logger that can be disabled in production.

```ts
// SECURE CODE
import logger from './logger'; // A simple wrapper around console.log

function userProfileReducer(state, action) {
  // ...
  if (action.type === 'USER_FETCH_SUCCESS') {
    // Our custom logger can be configured to be a no-op in production.
    // Or, we can be specific about what we log.
    logger.debug(`User with ID ${action.payload.user.id} fetched successfully.`);
    return { ...state, ...action.payload };
  }
  // ...
}

// logger.js
const logger = {
  debug: (...args) => {
    if (process.env.NODE_ENV !== 'production') {
      console.log(...args);
    }
  }
  // ... other methods like .error that might log in prod
};

```

**2. Blocklisting Sensitive Keys:** For services that automatically log Redux actions (like Sentry or LogRocket), they provide options to "blocklist" or "sanitize" certain keys from ever being sent.

```ts
// SECURE CONFIGURATION (Conceptual)
LogRocket.init('app/id', {
  redux: {
    // This tells the logger to replace the value of these keys
    // with '***' before sending the data.
    sanitizer: state => ({
      ...state,
      auth: {
        ...state.auth,
        token: '***',
      },
    }),
  },
});

```

This allows you to see the flow of actions without exposing the sensitive payload.

### 🤔 Reflective Questions

1. Your application experiences a critical error. To debug it, you need to know which user was affected. What is the most secure way to log user information in an error report sent to your server?
2. If an attacker has an XSS vulnerability on your page, they can often read the state from memory anyway. Does this mean trying to prevent state leakage via `console.log` or `window` is pointless? Why or why not?

### 📚 Further Reading

- **LogRocket:** [Redux Sanitization](https://www.google.com/search?q=https://docs.logrocket.com/docs/redux-advanced-setup%23sanitization) - A real-world example of configuring a logging service to protect sensitive state.

### 📝 Mini Task (Production)

You have a function that sends an error report to a tracking service. It currently sends the entire `auth` slice of your state.

```ts
// VULNERABLE CODE
import { reportError } from './errorService';

function handleApiError(error, authState) {
  // authState contains: { userId, userName, email, jwtToken }
  reportError(error, { context: { auth: authState } });
}

```

Your task: Refactor the `reportError` call to only include the `userId` in the context, preventing the user's name, email, and token from being sent.
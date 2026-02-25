💡 CI03.1.1: The Crown Jewels: What to Protect in Your State?

**Outline:**

- **The Threat (SEEI):** A "flat" view of state, where developers treat all data equally, failing to recognize that some pieces are highly sensitive and require special protection.
- **The Defense (PPP):** The principle of "Data Classification." Actively identifying, categorizing, and isolating sensitive data within your global state.
- **Your Mission (Production):** Analyze a sample application state and identify the sensitive fields.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is a **lack of awareness**. In a complex frontend application, the global state store (like Redux or Zustand) becomes a central hub for all application data. Developers, focused on functionality, often treat every piece of data the same. They fail to distinguish between a benign UI flag (like `isSidebarOpen`) and a highly sensitive piece of information like a JSON Web Token (JWT). This leads to sensitive data being handled insecurely by default.

**How it Works (The Attack Vector):**

Consider a typical state object for an e-commerce application. A developer might log this entire object to the console during debugging or send it to an error tracking service when an exception occurs.

```ts
// VULNERABLE STATE STRUCTURE
const state = {
  ui: {
    isCartOpen: false,
    theme: 'dark'
  },
  cart: {
    items: [{ id: 1, name: 'T-Shirt', price: 20 }]
  },
  // Sensitive data is mixed in with non-sensitive data
  auth: {
    user: {
      id: 123,
      name: 'Jane Doe',
      email: 'jane.doe@example.com', // PII
      address: '123 Main St'         // PII
    },
    token: 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...' // HIGHLY SENSITIVE
  }
};

```

If this entire `state` object is logged or leaked, the user's personal information (PII) and their session token are exposed. An attacker with that token could potentially take over the user's account.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"Know your crown jewels."** Before you can protect your state, you must classify it. You need to explicitly identify which pieces of information, if exposed, would cause harm to the user or the application. This process must be deliberate.

**Secure Code Implementation (Data Classification):**

Let's classify the data in our example state:

1. **Personally Identifiable Information (PII):** This is data that can be used to identify a specific individual. It requires a high level of protection.
    - `state.auth.user.name`
    - `state.auth.user.email`
    - `state.auth.user.address`
2. **Authentication/Authorization Tokens:** These are keys that grant access to the application. They are the most critical asset on the client-side.
    - `state.auth.token`
3. **Business Sensitive Data:** Data that might not be PII but is still confidential.
    - `state.cart.items` (Could reveal purchasing habits)
4. **Non-Sensitive UI State:** Data that has no security impact if exposed.
    - `state.ui.isCartOpen`
    - `state.ui.theme`

By performing this analysis, we now know that the entire `auth` slice and parts of the `cart` slice need special handling. We cannot treat them the same way we treat the `ui` slice.

### 🤔 Reflective Questions

1. Is a user's ID (`state.auth.user.id`) considered PII? Why or why not? What about when it's combined with other data?
2. Your application has a feature flag system. The state contains an object like `features: { canUseBetaFeature: true }`. Would you classify this as sensitive? What factors would influence your decision?

### 📚 Further Reading

- **OWASP:** [Top 10 - Sensitive Data Exposure](https://owasp.org/www-project-top-ten/2017/A3_2017-Sensitive_Data_Exposure) (Note: While this often refers to server-side, the principles of identifying sensitive data are universal).

### 📝 Mini Task (Production)

You are given the following Redux state object for a project management app.

```ts
const appState = {
  projects: {
    activeProjectId: 'proj-1',
    list: [{ id: 'proj-1', name: 'New Website' }]
  },
  tasks: {
    'proj-1': [{ id: 'task-1', title: 'Design Homepage' }]
  },
  userContext: {
    isOnline: true,
    preferences: { enableNotifications: true },
    apiKey: 'sk_live_f9a8b7c6d5e4...' // Secret API key for a third-party service
  },
  session: {
    userId: 'user-abc',
    isAuthenticated: true,
    lastLogin: '2023-10-27T10:00:00Z'
  }
};

```

Your task: List all the keys (e.g., `userContext.apiKey`) that you would classify as sensitive and require special protection.
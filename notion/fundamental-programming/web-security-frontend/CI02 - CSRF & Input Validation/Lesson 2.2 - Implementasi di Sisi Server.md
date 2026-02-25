💡 CI02.2.2: The Server as Guardian: Server-Side Implementation

**Outline:**

- **The Threat (SEEI):** A perfectly designed token pattern that is never actually enforced by the server, rendering it completely useless.
- **The Defense (PPP):** Implementing server-side logic to generate, store, and, most importantly, validate CSRF tokens for all state-changing requests.
- **Your Mission (Production):** Write a server-side middleware to protect a specific route.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **failed enforcement**. A developer might correctly implement the client-side part of the CSRF defense—fetching a token and sending it with requests—but forget to implement the validation logic on the server. The client is diligently sending the secret handshake, but the server isn't checking it. This is like having a bouncer at a club who checks IDs but never looks at the guest list.

**How it Works (The Attack Vector):**

An attacker creates their standard forged form on `evil.com`. This form does _not_ contain a CSRF token. The victim's browser submits the forged request to the vulnerable application.

```ts
// VULNERABLE SERVER CODE (Express.js)
// The developer has a route to get the token, but no validation middleware.
app.post('/api/settings/update', (req, res) => {
  // The server checks for a valid session cookie (authentication).
  if (!req.session.user) {
    return res.status(401).send('Unauthorized');
  }

  // FLAW: The server never checks for a CSRF token.
  // It trusts the request because the cookie is valid.
  updateUserSettings(req.session.user.id, req.body);
  res.send('Settings updated!');
});

```

The attacker's request succeeds because the server never validates the one piece of information that could have proven the request was forged.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The server must **"Generate on Read, Validate on Write."**

- **Generate on Read:** When a user makes a safe `GET` request to a page that contains a form, the server must generate a token and make it available.
- **Validate on Write:** For any state-changing request (`POST`, `PUT`, `DELETE`, `PATCH`), the server must have a mandatory middleware that validates the incoming token before the request handler is ever reached.

**Secure Code Implementation (Express.js Example):**

We'll use `express-session` for session management and Node's built-in `crypto` library.

**1. Generating and Providing the Token:**

```ts
import crypto from 'crypto';

// An endpoint for our client-side app to fetch the token
app.get('/api/csrf-token', (req, res) => {
  if (!req.session.csrf_token) {
    // Generate a token if one doesn't exist for this session
    req.session.csrf_token = crypto.randomBytes(32).toString('hex');
  }
  res.json({ csrf_token: req.session.csrf_token });
});

```

**2. The Validation Middleware:** This is the most critical piece. This middleware runs on every state-changing route.

```ts
const csrfProtection = (req, res, next) => {
  // We'll check for the token in a custom header
  const receivedToken = req.headers['x-csrf-token'];
  const sessionToken = req.session.csrf_token;

  if (!receivedToken || !sessionToken || receivedToken !== sessionToken) {
    return res.status(403).send('Forbidden: Invalid CSRF Token');
  }

  next(); // If tokens match, proceed to the actual route handler
};

```

**3. Applying the Middleware:** Now, we apply our middleware to all the routes that need protection.

```ts
// SECURE CODE
app.post('/api/settings/update', csrfProtection, (req, res) => {
  // This code will only run if the csrfProtection middleware calls next()
  updateUserSettings(req.session.user.id, req.body);
  res.send('Settings updated!');
});

app.delete('/api/posts/delete', csrfProtection, (req, res) => {
  // This is also protected
  deletePost(req.query.id);
  res.send('Post deleted!');
});

```

### 🤔 Reflective Questions

1. In our `csrfProtection` middleware, we checked for the token in a custom header (`x-csrf-token`). Why is this a common pattern for SPAs, as opposed to checking for it in the form body?
2. Should the `/api/csrf-token` GET endpoint itself be protected by the `csrfProtection` middleware? Why or why not?

### 📚 Further Reading

- **Express.js:** [csurf middleware](https://www.google.com/search?q=https://expressjs.com/en/resources/middleware/csurf.html) - A popular, pre-built library for handling CSRF in Express (though it's good to understand the manual implementation first).

### 📝 Mini Task (Production)

You have an Express.js application with three routes:

1. `GET /api/products` (fetches a list of products)
2. `POST /api/products` (creates a new product)
3. `GET /api/users/:id` (fetches a single user's public profile)

Your task: Which of these routes should the `csrfProtection` middleware be applied to? Explain your reasoning in a single sentence.
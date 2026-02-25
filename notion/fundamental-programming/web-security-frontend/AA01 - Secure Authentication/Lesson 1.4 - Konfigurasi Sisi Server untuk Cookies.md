💡 AA01-1.4: Server-Side Configuration for Cookies

**Outline:**

- **The Threat (SEEI):** Understanding that a secure frontend strategy is useless if the backend misconfigures the cookies.
- **The Defense (PPP):** Learning the exact server-side code and flags required to set truly secure cookies.
- **Your Mission (Production):** Time to write the backend code that forges the secure cookie.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The Vulnerability):** **Insecure Server Configuration.** Your React application is only a consumer of the security policies set by the backend. If your backend team provides an API that sets a cookie but forgets a critical flag like `HttpOnly` or `Secure`, then all the frontend security measures are fundamentally undermined. The threat is that a developer _thinks_ they are secure because they are "using cookies," but a misconfiguration leaves the door wide open.
    
- **How it Works (The Attack Vector):** A well-meaning but uninformed backend developer writes the following login endpoint.
    
    ```ts
    // VULNERABLE SERVER CODE (Node.js/Express)
    app.post('/api/login', (req, res) => {
      // ...user authentication logic...
      const token = generateJwtForUser(user);
    
      // PROBLEM: This cookie is missing critical security flags.
      // It is NOT HttpOnly, so XSS can steal it.
      // It is NOT Secure, so it can be sent over unencrypted HTTP.
      // It does NOT have SameSite, leaving it open to CSRF.
      res.cookie('auth_token', token); // This is dangerously simple.
    
      res.json({ message: 'Logged in!' });
    });
    
    ```
    
    The frontend developer implements the login flow perfectly. But because the cookie set by the server is insecure, an XSS attack can still read `document.cookie` and steal the `auth_token`. The point of failure was on the server.
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **The server is the ultimate authority on cookie security.** It is the server's responsibility to forge the cookie with all the necessary security attributes. As a frontend engineer, you must know what to ask for and how to verify it.
    
- **Secure Code Implementation:** Let's look at the robust, secure way to set a cookie on the server. We'll set both an access token (short-lived) and a refresh token (long-lived).
    
    ```ts
    // SECURE SERVER CODE (Node.js/Express)
    app.post('/api/login', (req, res) => {
      // ...user authentication logic...
      const accessToken = generateAccessToken(user);
      const refreshToken = generateRefreshToken(user);
    
      // Set the ACCESS token cookie
      res.cookie('access_token', accessToken, {
        httpOnly: true, // Cannot be accessed by client-side scripts
        secure: process.env.NODE_ENV === 'production', // Only sent over HTTPS
        sameSite: 'strict', // Strongest CSRF protection
        maxAge: 15 * 60 * 1000 // 15 minutes
      });
    
      // Set the REFRESH token cookie
      res.cookie('refresh_token', refreshToken, {
        httpOnly: true,
        secure: process.env.NODE_ENV === 'production',
        sameSite: 'strict',
        path: '/api/refresh', // IMPORTANT: Only send this cookie to the refresh endpoint
        maxAge: 7 * 24 * 60 * 60 * 1000 // 7 days
      });
    
      // Send back non-sensitive user data for the hybrid strategy
      res.json({ user: { username: user.name, role: user.role } });
    });
    
    ```
    

### 🧠 Real-World Case Study: "The Bank Teller's Instructions"

- **The Goal:** The server (the bank) needs to give the client (you) a secure way to prove your identity for future transactions.
    
- **Vulnerable Approach (Insecure Configuration):** The bank teller hands you a wad of cash (`token`) in a transparent, unsealed bag (`cookie with no flags`). Anyone can see it and snatch it. They didn't follow protocol.
    
- **Secure Approach (Secure Configuration):** The bank teller follows a strict protocol (`secure server code`):
    
    1. They place your short-term spending money (`access_token`) into a tamper-proof, locked pouch (`HttpOnly`, `Secure`, `SameSite`) that only works for 15 minutes.
    2. They give you a separate, highly-secure deposit slip (`refresh_token`) that can _only_ be used at the special "renew funds" window (`path: /api/refresh`). This slip is also in a locked pouch.
    3. They hand you a public-facing receipt (`user data in JSON body`) that you can keep in your wallet to remember your balance.
    
    The security comes from the teller (server) correctly using all the security features available.
    

### 🤔 Reflective Questions

1. In the secure example, why did we set `secure: process.env.NODE_ENV === 'production'`? What problem does this solve during development?
2. Why is it a good security practice to set a specific `path` for the refresh token cookie, like `/api/refresh`? How does this limit its exposure?
3. As a frontend developer, how can you verify that the cookies being set by your backend are actually secure? What tools would you use?

### 📚 Further Reading

- **Express.js Docs:** [res.cookie()](https://www.google.com/search?q=https://expressjs.com/en/api.html%23res.cookie) (The definitive guide for Express).
- **MDN Web Docs:** [SameSite cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite) (A deep-dive into `Strict`, `Lax`, and `None`).
- **IETF RFC 6265:** [The official spec for HTTP Cookies](https://datatracker.ietf.org/doc/html/rfc6265) (For when you want to go to the source).

### 📝 Mini-Task (Production)

You are the backend developer for your team. A frontend engineer has asked you to create a `/api/logout` endpoint.

Your task: Write the Express.js route handler for `POST /api/logout`. This handler must securely clear both the `access_token` and `refresh_token` cookies. Remember that to clear a cookie, you must send a cookie with the same name but with an empty value and an expiration date in the past (or `maxAge: 0`).

```ts
const express = require('express');
const app = express();

// Your task is to implement this endpoint.
app.post('/api/logout', (req, res) => {
  // How do you properly clear the two secure cookies we created earlier?
  // YOUR CODE GOES HERE
});

// Helper function to show it's working
app.listen(3001, () => console.log('Server running on port 3001'));

```
💡 AA01-2.1: Access & Refresh Token Architecture

**Outline:**

- **The Threat (SEEI):** Understanding the danger of a long-lived access token.
- **The Defense (PPP):** Introducing the two-token strategy to minimize the window of attack.
- **Your Mission (Production):** Time to design the JWT payloads for this new architecture.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The Vulnerability):** **Token Lifetime Exposure.** If you use a single, long-lived access token (e.g., valid for 7 days), its theft becomes a catastrophic event. An attacker who steals this token via XSS or other means gains access to the user's account for the _entire duration_ of the token's life. Revoking a specific stolen token is complex. You're left with a choice: either let the attacker have access or force a password change for the legitimate user, which is a terrible experience.
- **How it Works (The Attack Vector):**
    1. A user logs into your application.
    2. The server issues a single JWT, stored in a secure `HttpOnly` cookie, with a 7-day expiration.
    3. A different vulnerability exists in your stack—perhaps a compromised CDN, a rogue browser extension, or a server-side flaw that leaks the token.
    4. An attacker obtains this token.
    5. For the next 7 days, the attacker can make authenticated API requests on behalf of the user, stealing data, changing passwords, or performing malicious actions. The damage is continuous and hard to stop without disrupting the user.

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Minimize the lifespan of the key that unlocks data.** The solution is a two-token system that separates the permission to _access_ data from the permission to _renew access_.
    
    1. **Access Token:** A very short-lived JWT (e.g., 5-15 minutes). This is the key used to access protected API endpoints. Its payload contains user details, permissions, etc. Because its life is so short, its theft is far less damaging. By the time an attacker finds and uses it, it may have already expired.
    2. **Refresh Token:** A long-lived, opaque token (e.g., 7-30 days). This token's _only_ purpose is to be exchanged for a new access token. It is stored securely (in an `HttpOnly` cookie with a specific path) and is only ever sent to a single, highly-secured endpoint (e.g., `/api/refresh`).
- **Secure Code Implementation (Conceptual Flow):**
    
    ```
    // 1. User Logs In
    Client -> Server: POST /api/login with credentials
    Server -> Client:
        - Sets HttpOnly cookie for access_token (exp: 15 mins)
        - Sets HttpOnly cookie for refresh_token (exp: 7 days)
        - Returns user data in JSON body
    
    // 2. Client makes a normal API call
    Client -> Server: GET /api/profile
        - Browser automatically attaches access_token cookie.
        - Server validates access_token. If valid, returns data.
    
    // 3. Access Token Expires (after 15 mins)
    Client -> Server: GET /api/orders
        - Browser attaches expired access_token cookie.
        - Server rejects with 401 Unauthorized.
    
    // 4. Client's API interceptor catches 401
    Client -> Server: POST /api/refresh
        - Browser automatically attaches refresh_token cookie.
    Server -> Client:
        - Validates refresh_token.
        - Issues a NEW access_token.
        - Sets a new HttpOnly cookie for access_token (exp: 15 mins)
    
    // 5. Client retries the original failed request
    Client -> Server: GET /api/orders
        - Browser attaches the NEW access_token.
        - Server validates it and returns data.
    
    ```
    
    This entire refresh flow happens automatically and is invisible to the user.
    

### 🧠 Real-World Case Study: "The Day Pass vs. The Membership Card"

- **The Goal:** Securely access a members-only club (your application).
- **Vulnerable Approach (Single Long-Lived Token):** The club gives you a golden key that works for a whole year. If you lose that key on your first day, whoever finds it can enter the club for the rest of the year, and the club has no easy way of knowing the key was stolen.
- **Secure Approach (Two-Token Strategy):**
    - You are given a **Membership Card** (`refresh_token`). It's very valuable, has your photo, and you keep it securely in your wallet. It's valid for a whole year.
    - Every time you visit the club, you show your Membership Card at the front desk. The desk then gives you a paper **Day Pass** (`access_token`) that is only valid for the next hour. You use this Day Pass to get drinks, access the gym, etc.
    - If you accidentally drop your Day Pass, the damage is minimal. By the time someone finds it, it's likely already expired. To get a new one, they would need your secure Membership Card.

### 🤔 Reflective Questions

1. Why is it important that the refresh token is sent only to a single, dedicated refresh endpoint?
2. What are the UX and security trade-offs of setting a very short access token lifetime (e.g., 1 minute) vs. a slightly longer one (e.g., 30 minutes)?
3. Some systems implement "refresh token rotation," where a new refresh token is issued every time it's used. What security benefit does this provide?

### 📚 Further Reading

- **OWASP:** [JSON Web Token Cheat Sheet](https://www.google.com/search?q=https://cheatsheetseries.owasp.org/cheatsheets/JSON_Web_Token_Cheat_Sheet_for_Java.html)
- **Auth0 Blog:** [Using Refresh Tokens](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/)
- **IETF RFC 6749:** [The OAuth 2.0 Authorization Framework (Section 6 covers refreshing an access token)](https://www.google.com/search?q=https://datatracker.ietf.org/doc/html/rfc6749%23section-6)

### 📝 Mini-Task (Production)

You are designing the JWT architecture for a new application. Your task is to define the JSON payloads for both the access token and the refresh token.

- **Access Token:** This token will be read by your resource servers (`/api/orders`, `/api/profile`). What claims (e.g., `sub`, `exp`, `roles`, `permissions`) should it contain to be useful for authorization?
- **Refresh Token:** This token will only be read by the authentication server (`/api/refresh`). It doesn't need to contain detailed user permissions. What is the _minimal_ information it needs to securely identify the user and session it belongs to?

Write out the two JSON objects representing the payloads for each token. Justify your choice of claims for each.
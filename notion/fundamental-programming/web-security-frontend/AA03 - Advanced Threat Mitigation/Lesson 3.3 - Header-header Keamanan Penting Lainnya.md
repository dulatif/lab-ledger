💡 AE01-3.3: The Security Swiss Army Knife: More Essential Headers

**Outline:**

- **The Threat (SEEI):** A collection of diverse, subtle threats including MIME-type confusion attacks, leaking sensitive information through referrer URLs, and granting excessive browser feature permissions.
- **The Defense (PPP):** Implementing a suite of additional security headers that enforce stricter browser behaviors: `X-Content-Type-Options`, `Referrer-Policy`, and `Permissions-Policy`.
- **Your Mission (Production):** Time to write a `Permissions-Policy` header to lock down browser features for a specific type of application.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **Overly Permissive Browser Defaults.** By default, browsers sometimes try to be "helpful" in ways that can be exploited. They might guess the content type of a file if the server misconfigures it (`MIME-sniffing`), they send the full URL of the previous page when you navigate away (`Referrer leakage`), and they allow `iframes` to access powerful device features like the camera or microphone. These defaults violate the principle of least privilege.
- **How it Works (The Attack Vectors):**
    1. **`X-Content-Type-Options`:** An attacker tricks a user into uploading a file named `profile.jpg` that actually contains malicious JavaScript. Your server saves it and correctly serves it with a `Content-Type: image/jpeg` header. However, some old browsers might ignore this, "sniff" the file's content, see the script, and execute it when the "image" is loaded.
    2. **`Referrer-Policy`:** A user is on a password reset page: `https://site.com/reset?token=abc123xyz`. The page contains a link to an external site (e.g., a "Help" link). When the user clicks it, their browser sends a `Referer` header to the external site containing the full URL, `https://site.com/reset?token=abc123xyz`, leaking the secret token.
    3. **`Permissions-Policy`:** Your site embeds a third-party ad in an `iframe`. You have no `Permissions-Policy`. The ad's code is malicious and executes `navigator.geolocation.getCurrentPosition()`, asking the user for their location. The user sees a permission prompt that looks like it's from your site and approves it. The ad has now stolen the user's location data.

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Enforce the principle of least privilege.** Tell the browser to disable any features or behaviors your application doesn't strictly need.
- **Secure Code Implementation (Server-Side Headers):**
    1. **`X-Content-Type-Options`**
        
        - This header is simple. It has only one value. It tells the browser to never second-guess the `Content-Type` header sent by the server.
        
        ```ts
        res.setHeader("X-Content-Type-Options", "nosniff");
        ```
        
    2. **`Referrer-Policy`**
        
        - This header controls how much referrer information is sent. A good, modern default is `strict-origin-when-cross-origin`.
        
        ```ts
        res.setHeader("Referrer-Policy", "strict-origin-when-cross-origin");
        
        ```
        
        - This means: when navigating on your own site, send the full URL. When navigating to a different site, only send the origin (e.g., `https://your-site.com/`).
    3. **`Permissions-Policy`** (formerly `Feature-Policy`)
        
        - This header allows you to explicitly disable browser features for your site and any embedded `iframes`.
        
        ```ts
        // A very strict policy: disable everything by default, then enable nothing.
        res.setHeader("Permissions-Policy", "camera=(), microphone=(), geolocation=()");
        
        ```
        
        - You can also be more granular, allowing features for your own origin but not for `iframes`: `camera=(self), microphone=()`

### 🧠 Real-World Case Study: "The Leaky URL"

- **The Goal:** Prevent leaking sensitive URL parameters to third parties. Many web applications, especially single-page apps, use URL parameters or fragments to hold session information or identifiers.
- **The System:** A web analytics service. When a user navigates from your site (`yoursite.com`) to an external site (`othersite.com`), the analytics script on `othersite.com` can read the `document.referrer`.
- **Vulnerable Approach (Default Policy):**
    - User is on `https://yoursite.com/edit-document/d-SECRET_DOC_ID`.
    - They click a link to `https://othersite.com`.
    - `othersite.com` receives a `Referer` header with the value `https://yoursite.com/edit-document/d-SECRET_DOC_ID`. The secret document ID has been leaked.
- **Secure Approach (Using `strict-origin-when-cross-origin`):**
    - Same user, same starting page.
    - They click the same link.
    - `othersite.com` receives a `Referer` header with the value `https://yoursite.com/`. The path and parameters have been stripped. The data is safe.

### 🤔 Reflective Questions

1. If you set `X-Content-Type-Options: nosniff`, what is the critical importance of ensuring your server _always_ sends the correct `Content-Type` header for all assets?
2. What is a scenario where you might want to use a more restrictive `Referrer-Policy` like `no-referrer`?
3. Why is `Permissions-Policy` becoming increasingly important as web browsers add more powerful APIs that interact with a user's hardware?

### 📚 Further Reading

- **MDN Web Docs:** [X-Content-Type-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options)
- **MDN Web Docs:** [Referrer-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy)
- **MDN Web Docs:** [Permissions-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Permissions-Policy)

### 📝 Mini-Task (Production)

You are building a simple, static blog website. The site has no user comments, no embedded media from other sites (except your own images), and does not need to access any device hardware.

Your mission: Write the most restrictive, secure `Permissions-Policy` header you can for this blog. Your goal is to disable as many features as possible to minimize the attack surface. You only need to list 3-4 features in your policy as an example (e.g., `camera`, `microphone`, `geolocation`, `payment`).
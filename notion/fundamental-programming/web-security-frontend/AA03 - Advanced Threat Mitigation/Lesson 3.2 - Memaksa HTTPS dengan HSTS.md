💡 AE01-3.2: No Plaintext Allowed: Forcing HTTPS with HSTS

**Outline:**

- **The Threat (SEEI):** SSL Stripping attacks, a type of Man-in-the-Middle (MITM) attack where an attacker intercepts an initial HTTP request and prevents the browser from upgrading to a secure HTTPS connection.
- **The Defense (PPP):** Implementing the `Strict-Transport-Security` (HSTS) header to tell the browser that it must _never_ communicate with your site over an insecure connection.
- **Your Mission (Production):** Time to analyze the directives of an HSTS header and understand the implications of the `preload` directive.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **The First Insecure Request.** Even if your website is perfectly configured to redirect all HTTP traffic to HTTPS, there's a small window of opportunity for an attacker. When a user types `your-site.com` into their browser, the very first request is typically made over `http://`. An attacker on the same network (e.g., public Wi-Fi) can intercept this request. This is called an **SSL Stripping** attack. The attacker forwards the request to your server over HTTPS, gets the secure response, and then serves a non-secure HTTP version back to the user. The user's browser never sees the redirect, the lock icon never appears, and the entire session is now in plaintext, visible to the attacker.
- **How it Works (The Attack Vector):**
    1. **User:** Types `my-bank.com` into their browser. Browser sends `GET <http://my-bank.com`>.
    2. **Attacker:** Intercepts this request on the local network.
    3. **Attacker to Server:** Attacker sends `GET <https://my-bank.com`> to the real bank server.
    4. **Server to Attacker:** Server responds with the secure homepage over HTTPS.
    5. **Attacker to User:** Attacker strips all HTTPS elements from the response and forwards it to the user over HTTP.
    6. **Result:** The user is interacting with a non-secure version of the bank's website, and the attacker is in the middle, reading all traffic.

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Once a user has visited your site securely, never allow their browser to make an insecure request to it again.** HSTS enforces this rule.
    
- **Secure Code Implementation (Server-Side Header):** The `Strict-Transport-Security` header is the defense. Once a browser receives this header over a secure connection, it makes a note. For the duration specified, it will refuse to make any insecure (`http://`) requests to that domain and will automatically convert them to `https://` before they are even sent.
    
    ```ts
    // In your Node.js/Express server
    app.use((req, res, next) => {
      // The header MUST be served over an existing HTTPS connection.
      // It has no effect over HTTP.
    
      const oneYearInSeconds = 31536000;
    
      // A strong HSTS policy:
      res.setHeader(
        "Strict-Transport-Security",
        `max-age=${oneYearInSeconds}; includeSubDomains; preload`
      );
    
      next();
    });
    
    ```
    
    - **`max-age=<seconds>`:** (Required) The time in seconds that the browser should remember this policy. A year (`31536000`) is a common value.
    - **`includeSubDomains`:** (Optional, but recommended) The policy applies to all subdomains (e.g., `api.your-site.com`, `blog.your-site.com`) as well.
    - **`preload`:** (Optional) An instruction that gives Google permission to hard-code your site into Chrome's source code as HTTPS-only. This protects even the very first visit a user ever makes.

### 🧠 Real-World Case Study: "The Preload List"

- **The Goal:** Protect users who have never visited your site before from SSL stripping. HSTS works after the first successful HTTPS visit, but what about the very first one?
- **The System:** The HSTS Preload list, maintained by Google and shared by all major browsers (Chrome, Firefox, Safari, Edge).
- **The Implementation:** If you set a strong HSTS header (long `max-age`, `includeSubDomains`, and the `preload` directive), you can submit your domain to `hstspreload.org`.
- **The Result:** After acceptance, your domain is baked directly into the browser. Now, when a user types `your-site.com` for the very first time, the browser checks its internal list. It sees your domain and says, "Nope, this site is on the preload list. I will not make an `http://` request." It automatically changes the request to `https://your-site.com` _before_ it ever leaves the browser. This closes the "first visit" vulnerability completely.

### 🤔 Reflective Questions

1. What is the potential danger of setting a short `max-age` for your HSTS policy?
2. If you add `your-site.com` to the HSTS preload list with `includeSubDomains`, what happens to the internal testing server at `dev.your-site.com` that was running on HTTP?
3. Why must the `Strict-Transport-Security` header itself be ignored by the browser if it's received over an insecure HTTP connection?

### 📚 Further Reading

- **MDN Web Docs:** [Strict-Transport-Security](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security)
- **OWASP:** [HSTS Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html)
- **HSTS Preload List Submission Site:** [hstspreload.org](https://hstspreload.org/)

### 📝 Mini-Task (Production)

You are a DevOps engineer setting up a new website. Your security lead has given you the following requirements for HSTS:

1. The policy must be active for at least 6 months.
2. The policy must apply to `www.example.com` and `api.example.com`.
3. The site should be made eligible for inclusion in the browser preload lists.

Your mission: Write the full, single-line `Strict-Transport-Security` header value that meets all of these requirements. (Note: 6 months is roughly 15,552,000 seconds).
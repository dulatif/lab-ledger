💡 AE01-2.4: The DOM's New Bouncer: Trusted Types

**Outline:**

- **The Threat (SEEI):** DOM XSS is pervasive because dangerous APIs (sinks) accept simple strings. This makes it hard to audit large applications to ensure that no untrusted strings ever reach a sink.
- **The Defense (PPP):** Introducing Trusted Types, a new browser security feature that locks down these dangerous sinks. They will no longer accept strings, only special "Trusted" objects created by a security policy you define.
- **Your Mission (Production):** Time to write a basic Trusted Types policy and see how it blocks unsafe DOM manipulation.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **The Stringly-Typed Nature of Web APIs.** The root cause of DOM XSS is that the web platform was built with APIs like `innerHTML` that accept a string and interpret it as code. As applications grow, it becomes nearly impossible to manually track every single line of code and guarantee that no user-controllable string can ever reach one of these sinks. A single mistake anywhere in the codebase can lead to a vulnerability. This is a massive attack surface.
    
- **How it Works (The Attack Vector):** In a large application with hundreds of developers, one developer might add a new feature that uses `innerHTML` with what they _think_ is safe, internal data.
    
    ```ts
    // A developer adds a "feature flag" to display a banner
    const bannerData = getBannerDataFromApi(); // This is assumed to be safe
    document.querySelector('.banner').innerHTML = bannerData.htmlContent;
    
    ```
    
    Months later, another developer changes the `getBannerDataFromApi` function to include a piece of data from the URL for A/B testing, accidentally introducing a path for user input to reach the `innerHTML` sink. The original developer is unaware of this change. The vulnerability is now hidden deep within the application logic.
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Make security the default. Instead of trying to find every dangerous data flow, lock down the dangerous APIs themselves.** Trusted Types changes the default behavior of sinks from "insecure" (accepts strings) to "secure" (throws an error for strings).
    
- **Secure Code Implementation:Step 1: Enforce with a CSP Header** You enable Trusted Types via a Content Security Policy header sent from your server.
    
    ```ts
    Content-Security-Policy:
      require-trusted-types-for 'script';
      trusted-types my-app-policy;
    
    ```
    
    This tells the browser: "For sinks that create scripts (like `innerHTML`), you must use Trusted Types. And the only policy allowed to create these types is named 'my-app-policy'."
    
    **Step 2: Define a Policy** In your JavaScript, you define the policy and how it creates trusted objects. This becomes the _only_ place in your app that can generate HTML for a sink.
    
    ```ts
    // This code runs once when the app starts.
    if (window.trustedTypes && window.trustedTypes.createPolicy) {
      const policy = trustedTypes.createPolicy('my-app-policy', {
        // This function MUST return a safe, sanitized string.
        createHTML: (untrustedString) => {
          // We use a sanitizer like DOMPurify here.
          return DOMPurify.sanitize(untrustedString);
        }
        // You can also define createScript, createScriptURL, etc.
      });
    }
    
    ```
    
    **Step 3: Use the Policy**
    
    ```ts
    // Now, this is how you interact with a locked-down sink.
    
    // THIS WILL THROW AN ERROR. 'innerHTML' no longer accepts strings!
    // document.querySelector('.banner').innerHTML = "<b>Hello!</b>";
    
    // You MUST use your policy to create a TrustedHTML object.
    const trustedHtml = policy.createHTML("<b>Hello!</b>");
    
    // THIS WORKS!
    document.querySelector('.banner').innerHTML = trustedHtml;
    
    ```
    
    Now, instead of auditing hundreds of `innerHTML` calls, you only need to audit the logic inside your one `createHTML` function.
    

### 🧠 Real-World Case Study: "The VIP Club"

- **The Goal:** A nightclub wants to ensure only approved guests can enter.
- **The System:**
    - **The Club Entrance:** A dangerous sink like `innerHTML`.
    - **Guests:** Strings of data.
- **Vulnerable Approach (No Trusted Types):** The bouncer lets anyone who approaches the door inside. They have to individually inspect every person in the line to see if they look suspicious. If they miss one dangerous person, the club's security is compromised.
- **Secure Approach (With Trusted Types):** The club owner installs a new rule: The main entrance is now locked. The bouncer (`innerHTML`) is not allowed to open it for anyone. To get in, you must first go to a special, heavily-guarded VIP check-in desk at the front (`the Trusted Types policy`). This desk will inspect you, and if you are safe, they will give you a special, unforgeable wristband (`TrustedHTML` object). The bouncer at the main door is now only allowed to open the door for people with this specific wristband. The security effort is now focused entirely on the VIP desk, which is much easier to manage.

### 🤔 Reflective Questions

1. What is the primary benefit of Trusted Types in a large, legacy application with many existing uses of `innerHTML`?
2. Trusted Types can be deployed in "report-only" mode first. Why is this a critical feature for adoption?
3. How does the concept of Trusted Types mirror the type systems in languages like TypeScript or Rust, and how does that contribute to security?

### 📚 Further Reading

- **Google's web.dev:** [Prevent DOM-based cross-site scripting vulnerabilities with Trusted Types](https://web.dev/articles/trusted-types)
- **MDN Web Docs:** [Trusted Types API](https://developer.mozilla.org/en-US/docs/Web/API/Trusted_Types_API)
- **W3C Spec:** [The official Trusted Types specification](https://w3c.github.io/trusted-types/dist/spec/)

### 📝 Mini-Task (Production)

Assume a Content Security Policy `require-trusted-types-for 'script'` is active. You are given the following simple Trusted Types policy:

```ts
const myPolicy = trustedTypes.createPolicy('default', {
  createHTML: (str) => {
    // A very simple (and not very good) sanitizer.
    return str.replace(/</g, '&lt;');
  }
});

```

Your mission: For each of the following lines of code, state whether it will succeed or throw a `TypeError`.

1. `el.innerHTML = "Hello, World!";`
2. `el.innerHTML = myPolicy.createHTML("<b>Hello, World!</b>");`
3. `const data = "<img src=x>"; el.innerHTML = data;`
4. `const safeData = myPolicy.createHTML("<img src=x>"); el.innerHTML = safeData;`
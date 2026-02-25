💡 AE01-2.2: Identity Theft for Objects: An Intro to DOM Clobbering

**Outline:**

- **The Threat (SEEI):** Understanding how an attacker can inject HTML that creates global JavaScript variables, potentially overwriting legitimate variables and objects your application relies on.
- **The Defense (PPP):** Adopting coding patterns that avoid reliance on global variables for security decisions and properly sanitizing HTML input.
- **Your Mission (Production):** Time to craft an HTML payload that clobbers a global variable and bypasses a security check.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **DOM Clobbering** is a legacy browser behavior where creating a DOM element with an `id` or `name` attribute can automatically create a corresponding global variable on the `window` object. An attacker can exploit this by injecting specific HTML to "clobber" (overwrite) a global variable your application expects to exist, or not exist. This can bypass security checks, break application logic, or lead to XSS.
    
- **How it Works (The Attack Vector):** An application has some security-sensitive logic that it only runs if a global configuration object is present.
    
    ```ts
    // VULNERABLE CODE
    // This code is supposed to be safe because 'adminConfig' is only defined for admins.
    if (window.adminConfig) {
      // Load sensitive admin scripts or data
      loadAdminPanel(window.adminConfig.api_key);
    }
    
    ```
    
    An attacker finds a place where they can inject HTML (like a user profile bio). They inject the following:
    
    ```html
    <form id="adminConfig">
      <input name="api_key" value="dummy_value">
    </form>
    
    ```
    
    Due to DOM clobbering, the browser does the following:
    
    1. It creates a global variable `window.adminConfig` which now points to the `<form>` element.
    2. The check `if (window.adminConfig)` is now `true` because the DOM element exists.
    3. The code then tries to access `window.adminConfig.api_key`. The browser helpfully provides a reference to the `<input>` element with `name="api_key"`.
    4. The `loadAdminPanel` function is called with a reference to an HTML element instead of the expected configuration object, likely causing an error, but potentially bypassing the initial security check.

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Do not use the existence of global variables as a security control.** Always validate the _type_ and _properties_ of an object, not just its truthiness. And, as always, sanitize any user-provided HTML before injecting it into the DOM.
    
- **Secure Code Implementation:** The JavaScript check needs to be more robust.
    
    ```ts
    // SECURE CODE
    
    // Check not just for existence, but for a specific property that a DOM element wouldn't have.
    if (window.adminConfig && typeof window.adminConfig === 'object' && window.adminConfig.isLegitConfig === true) {
      // This check is much harder to bypass with DOM clobbering.
      loadAdminPanel(window.adminConfig.api_key);
    }
    
    ```
    
    The best defense is to avoid global variables altogether by using modern JavaScript modules (`import`/`export`).
    
    ```
    // EVEN BETTER - NO GLOBALS
    import { adminConfig } from './config';
    
    if (adminConfig) { // This can't be clobbered by the DOM
      loadAdminPanel(adminConfig.api_key);
    }
    
    ```
    

### 🧠 Real-World Case Study: "Vulnerable vs. Secured Code"

- **The Goal:** A web page has a global variable `adsConfig` that controls ad loading. If it's undefined, no ads are loaded.
- **Vulnerable Approach:** The code simply checks `if (window.adsConfig)`. An attacker who can inject HTML could add `<div id="adsConfig"></div>` to a comment. This creates a global variable, `window.adsConfig` becomes a truthy `HTMLDivElement`, and the application might break when it tries to read properties like `adsConfig.placementId`.
- **Secure Approach (The Robust Way):** The code is changed to check `if (window.adsConfig && window.adsConfig.isLoaded)`. The attacker's injected `div` does not have an `isLoaded` property, so the check fails. The application remains stable, and the intended logic is preserved. The best approach is to refactor the code to use modules and avoid the global `adsConfig` entirely.

### 🤔 Reflective Questions

1. How does using modern JavaScript frameworks like React, which encourage component-scoped state and props over global variables, naturally reduce the risk of DOM Clobbering?
2. DOM clobbering can also affect form submissions. If you have `<form id="submit">`, what might happen when you try to call `myForm.submit()` in JavaScript?
3. Why is it especially dangerous to use user-controlled strings to dynamically create `id` attributes on DOM elements?

### 📚 Further Reading

- **PortSwigger:** [What is DOM clobbering?](https://portswigger.net/web-security/dom-based/dom-clobbering)
- **OWASP:** [DOM Clobbering](https://www.google.com/search?q=https://owasp.org/www-project-proactive-controls/v3/en/c5-validate-all-inputs) (Mentioned under Input Validation)
- **Blog Post by Cure53:** [DOM Clobbering Strikes Back](https://www.google.com/search?q=https://cure53.de/blog/2016/12/14/dom-clobbering-strikes-back.html)

### 📝 Mini-Task (Production)

You are given the following vulnerable JavaScript code that decides whether to show a "Premium User" banner.

```ts
// This 'premiumUser' object is only supposed to be defined when a premium user is logged in.
// if (window.premiumUser) {
//   document.getElementById('banner').style.display = 'block';
// }

// Your vulnerable HTML might look like this:
// <div id="user-comments"><!-- user-controlled HTML is injected here --></div>

```

Your mission:

1. Write a single line of HTML that, when injected into the `user-comments` div, would clobber the `premiumUser` global variable and cause the banner to be shown incorrectly.
2. Rewrite the JavaScript `if` condition to be secure against this DOM clobbering attack.
💡 AE01-2.1: The DOM as a Battlefield: Advanced DOM-based XSS

**Outline:**

- **The Threat (SEEI):** Understanding how DOM-based XSS is a pure client-side attack, where the server is completely unaware of the malicious payload.
- **The Defense (PPP):** Learning to treat all client-side data sources (like URL fragments) as untrusted input and using safe DOM manipulation APIs.
- **Your Mission (Production):** Time to find and fix a DOM-based XSS vulnerability in a piece of JavaScript code.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **DOM-based Cross-Site Scripting (XSS)** is a subtle and dangerous variant of XSS. Unlike other XSS attacks where the server might reflect a payload, a DOM-based XSS attack happens entirely in the victim's browser. It occurs when a client-side script reads data from a user-controllable source (like the URL) and writes it back into the DOM in an unsafe way, causing malicious code to execute. The server might serve a completely clean, static HTML page, yet the application can still be vulnerable.
    
- **How it Works (The Attack Vector):** A web page wants to welcome a user by name, pulling the name from a URL parameter.
    
    ```ts
    // VULNERABLE CODE
    // The URL is: <https://example.com/?name=><img src=x onerror=alert('XSS')>
    
    // A script on the page reads the 'name' parameter.
    const userName = new URLSearchParams(window.location.search).get('name');
    
    // The script then writes this value directly into the page's HTML.
    // This is a dangerous "sink".
    document.getElementById('welcome-message').innerHTML = `Welcome, ${userName}!`;
    
    ```
    
    The server only sees a request for `/`. The payload, `<img src=x onerror=alert('XSS')>`, is never sent to the server. It exists only in the client's URL. The browser's own JavaScript engine reads this payload and injects it into the DOM via `innerHTML`, causing the script to execute.
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **NEVER use APIs that parse HTML (like `innerHTML`) with data that originated from any untrusted source.** Treat data from URLs (`location.search`, `location.hash`), post messages (`onmessage`), and even browser storage (`localStorage`) as potentially malicious.
    
- **Secure Code Implementation:** The fix is to use APIs that treat the data as text, not as HTML.
    
    ```ts
    // SECURE CODE
    // The URL is the same malicious one.
    
    const userName = new URLSearchParams(window.location.search).get('name');
    
    // We use .textContent, which does NOT parse HTML.
    // It will render the malicious string as plain text on the screen.
    document.getElementById('welcome-message').textContent = `Welcome, ${userName}!`;
    
    ```
    
    By using `.textContent`, the browser will literally display the string `<img src=x onerror=alert('XSS')>` on the page. The HTML tags are not interpreted, and the script does not execute.
    

### 🧠 Real-World Case Study: "Vulnerable vs. Secured Code"

- **The Goal:** Displaying a search query from the URL back to the user on the results page.
- **Vulnerable Approach:** Reading from `location.hash` and writing with `innerHTML`.
    - **URL:** `https://search-site.com/#<script>alert('stolen-cookies')</script>`
    - **Code:** `resultsContainer.innerHTML = 'Showing results for: ' + location.hash.substring(1);`
    - **Result:** The script executes. The server logs show no malicious payload, making the attack difficult to detect from the backend.
- **Secure Approach (The Safe Way):** Reading from `location.hash` and writing with `textContent`.
    - **URL:** Same malicious URL.
    - **Code:** `resultsContainer.textContent = 'Showing results for: ' + location.hash.substring(1);`
    - **Result:** The page safely displays "Showing results for: `<script>alert('stolen-cookies')</script>`". The attack is neutralized.

### 🤔 Reflective Questions

1. What are some other client-side data sources, besides the URL, that could be a source for DOM-based XSS if not handled carefully?
2. How does React's default behavior of escaping JSX expressions (e.g., `<div>{userInput}</div>`) protect developers from this entire class of vulnerability?
3. Why is DOM-based XSS often harder for automated security scanners to detect compared to reflected or stored XSS?

### 📚 Further Reading

- **OWASP:** [DOM based XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html)
- **PortSwigger:** [What is DOM-based XSS?](https://portswigger.net/web-security/cross-site-scripting/dom-based)
- **MDN Web Docs:** [Element.innerHTML](https://www.google.com/search?q=https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML%23security_considerations) (Read the security warning!)

### 📝 Mini-Task (Production)

You are given a small JavaScript snippet for a "404 Not Found" page. It takes the path that was not found from the URL and displays it to the user.

```ts
// A user lands on <https://example.com/this/page/is/missing>

// This function is supposed to display a helpful error message.
function showNotFoundMessage() {
  const missingPath = window.location.pathname;
  const errorContainer = document.getElementById('error-message');

  // This line is vulnerable!
  errorContainer.innerHTML = `Sorry, the path <code>${missingPath}</code> was not found.`;
}

showNotFoundMessage();

```

Your mission:

1. Construct a URL path that would trigger an XSS alert on this page.
2. Refactor the `showNotFoundMessage` function to be secure against this attack.
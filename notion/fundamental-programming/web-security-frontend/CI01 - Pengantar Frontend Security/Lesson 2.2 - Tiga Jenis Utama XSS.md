💡 CI01.2.2: The XSS Trinity: Stored, Reflected, and DOM-based

**Outline:**

- **The Threat (SEEI):** Understanding the three primary ways XSS attacks are delivered.
- **The Defense (PPP):** Applying the right defensive mindset for each attack vector.
- **Your Mission (Production):** Classify different XSS attack scenarios.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

Not all XSS attacks are the same. They are classified based on how the malicious payload is delivered to the victim's browser. Understanding these categories is crucial because it helps you know where to look for vulnerabilities in your own code. The three main types are **Stored XSS**, **Reflected XSS**, and **DOM-based XSS**.

**How it Works (The Attack Vectors):**

1. **Stored XSS (The Time Bomb):**
    - **Elaborate:** This is the most damaging type. The attacker's malicious script is permanently stored on the target server—in a database, a comment field, a user profile, etc. When a victim browses to the affected page, the server sends the stored script along with the legitimate content, and the victim's browser executes it.
        
    - **Exemplify:**
        
        ```tsx
        // A user profile page component
        const UserBio = ({ bioFromDatabase }) => {
          // If 'bioFromDatabase' contains a script, every visitor gets attacked.
          return <div dangerouslySetInnerHTML={{ __html: bioFromDatabase }} />;
        };
        
        ```
        
    - **Analogy:** Stored XSS is like leaving a malicious note on a public bulletin board for every future visitor to read and fall victim to.
        
2. **Reflected XSS (The Boomerang):**
    - **Elaborate:** The injected script is delivered via the HTTP request itself, typically as a URL parameter. The server-side application then "reflects" the script back in the HTML response. To succeed, the attacker must trick the victim into clicking a specially crafted link.
        
    - **Exemplify:**
        
        ```tsx
        // A search results page
        // URL: [yoursite.com/search?query=](<https://yoursite.com/search?query=>)<script>alert('XSS')</script>
        const SearchResults = ({ query }) => {
          // The server takes 'query' from the URL and puts it in the page.
          return <div dangerouslySetInnerHTML={{ __html: `Showing results for: ${query}` }} />;
        };
        
        ```
        
    - **Analogy:** Reflected XSS is like tricking someone into asking a librarian (the server) a question that contains a malicious phrase. The librarian, in answering, reads the malicious phrase back out loud.
        
3. **DOM-based XSS (The Client-Side Sneak):**
    - **Elaborate:** This is a subtle variation where the entire attack happens on the client side. The server is not involved in reflecting the payload. A client-side script reads data from a source controlled by the user (like the URL hash `window.location.hash`) and unsafely writes it back into the page's DOM.
        
    - **Exemplify:**
        
        ```tsx
        // A legacy script on the page, outside of React's control
        const hash = window.location.hash.substring(1);
        // DANGER: Unsafely writing user-controllable data into the DOM.
        document.getElementById('welcome-message').innerHTML = `Welcome, ${hash}`;
        
        ```
        
    - **Analogy:** DOM-based XSS is like giving someone a complex origami paper (the web page) with instructions. The instructions trick the person into folding the paper in a way that reveals a hidden, malicious message, all without ever talking back to the person who gave them the paper.
        

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The defense remains **context-aware output encoding**, but your focus changes:

- **For Stored & Reflected XSS:** Your defense is focused on how your server-side template or your client-side framework (like React) renders data. **Always encode data right before it's rendered into HTML.**
- **For DOM-based XSS:** Your defense is focused on your client-side JavaScript. **Avoid dangerous DOM manipulation functions like `innerHTML`**. Instead, use safe alternatives like `textContent` that don't interpret HTML.

### 🤔 Reflective Questions

1. Why is Stored XSS generally considered more dangerous than Reflected XSS?
2. Modern single-page applications (SPAs) built with React or similar frameworks are more susceptible to which type of XSS, and why?

### 📚 Further Reading

- **OWASP:** [Types of Cross-Site Scripting](https://owasp.org/www-community/Types_of_Cross-Site_Scripting) - A concise summary of the different XSS vectors.
- **MDN:** [`element.innerHTML`](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML) vs. [`node.textContent`](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent) - Understand the difference between these critical DOM properties.

### 📝 Mini Task (Production)

For each scenario below, identify whether it describes **Stored XSS**, **Reflected XSS**, or **DOM-based XSS**.

1. An attacker sends an email to a victim with a link: `http://yourapp.com/settings?lang=<script>...<script>`. When the user clicks it, the settings page shows a "Language not found" error that includes the malicious script, which then executes.
2. A developer writes a feature that uses JavaScript to read the document's referrer (`document.referrer`) and displays it on the page for analytics using `document.write()`. An attacker can get a user to click a link on their malicious site that leads to the vulnerable page, executing a script.
3. A hacker signs up for a forum and sets their username to a malicious JavaScript payload. Every time another user views a thread where the hacker has posted, the script runs in their browser.
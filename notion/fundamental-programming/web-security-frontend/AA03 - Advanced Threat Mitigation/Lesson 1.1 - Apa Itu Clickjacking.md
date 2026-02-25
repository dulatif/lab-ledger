💡 AE01-1.1: The Invisible Threat: What is Clickjacking?

**Outline:**

- **The Threat (SEEI):** Understanding how an attacker can load your website in a transparent `iframe` on their malicious page to trick users into performing actions they don't intend to.
- **The Defense (PPP):** Introducing the core principle of defense: controlling which other websites are allowed to embed yours in a frame.
- **Your Mission (Production):** Time to analyze a real-world scenario and explain the potential damage.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **UI Redressing**, more commonly known as **Clickjacking**, is a deceptive attack where a user is tricked into clicking on something different from what they perceive. The attacker achieves this by displaying an invisible page or UI element in an `iframe` on top of a visible page. The user _thinks_ they are clicking a harmless button (e.g., "Play Game"), but they are actually clicking an invisible button on the hidden, legitimate site (e.g., "Transfer Funds" or "Delete Account").
- **How it Works (The Attack Vector):**
    1. The attacker creates a malicious webpage with an enticing button, like "Click here for a free prize!"
    2. They then embed your legitimate website (e.g., `your-bank.com`) in an `iframe` on their page.
    3. Using CSS, they make the `iframe` completely transparent (`opacity: 0`).
    4. They precisely position the transparent `iframe` so that a critical button on your site (like the "Confirm Transfer" button) is located directly on top of their visible "free prize" button.
    5. The victim, who is already logged into `your-bank.com`, visits the attacker's page. They see the prize button and click it.
    6. Their click passes through the visible button and is registered by the invisible `iframe`, effectively clicking the "Confirm Transfer" button on your banking website. The user has been "jacked" into performing a click they never intended.

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Your website must explicitly tell browsers whether it allows itself to be embedded inside `iframes`, `<frames>`, `<objects>`, or `<embed>` tags on other sites.** By default, browsers will allow this framing, so you must opt-out. If you don't need to be framed, you must forbid it.
    
- **Secure Code Implementation (Conceptual):** The defense against clickjacking is not implemented in the frontend JavaScript code, but rather through **HTTP Response Headers** sent from the server. These headers are instructions for the browser. The two primary headers for this are:
    
    1. `X-Frame-Options` (the older, widely supported method).
    2. `Content-Security-Policy: frame-ancestors` (the modern, more flexible method).
    
    We will dive deep into how to implement these in the next lessons. The key takeaway for now is that you prevent this attack by configuring your web server, not by writing React code.
    

### 🧠 Real-World Case Study: "The Hidden Contract"

- **The Goal:** Get a person's signature.
- **The System:** A clipboard with documents.
    - **Vulnerable Approach (No Protection):** A scammer approaches you on the street with a clipboard that has a fun survey on top asking, "Do you like puppies?" You happily sign your name at the bottom. Later, the scammer removes the survey page, revealing that underneath was a legally binding contract to give them your car, with your signature perfectly placed on the signature line. The survey was the visible UI; the contract was the invisible `iframe`.
    - **Secure Approach (Clickjacking Protection):** The contract is printed on special "no-copy" paper that becomes black if another piece of paper is placed on top of it (`X-Frame-Options: DENY`). It is now impossible to hide the contract's true nature under a fun survey. The document itself enforces how it can be presented.

### 🤔 Reflective Questions

1. Why is clickjacking particularly dangerous for actions that don't have a final confirmation step (e.g., a "like" button vs. a "delete account" button that asks "Are you sure?")?
2. Can an attacker use clickjacking to steal a user's password? Why or why not?
3. Besides financial or social media sites, what other types of websites would be high-value targets for clickjacking attacks?

### 📚 Further Reading

- **OWASP:** [Clickjacking](https://owasp.org/www-community/attacks/Clickjacking)
- **MDN Web Docs:** [Clickjacking](https://developer.mozilla.org/en-US/docs/Glossary/Clickjacking)
- **PortSwigger:** [What is Clickjacking?](https://portswigger.net/web-security/clickjacking)

### 📝 Mini-Task (Production)

Imagine a social media website that has a "Block User" button on every profile page. Describe, in a few sentences, how an attacker could use clickjacking to trick a user, Alice, into blocking her friend, Bob. What would the attacker's page look like, and what would Alice think she is doing?
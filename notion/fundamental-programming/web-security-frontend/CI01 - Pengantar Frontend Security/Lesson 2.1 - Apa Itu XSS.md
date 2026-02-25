💡 CI01.2.1: The Digital Hijack: What is Cross-Site Scripting?

**Outline:**

- **The Threat (SEEI):** Understanding how XSS turns a trusted website into a weapon.
- **The Defense (PPP):** Learning how output encoding is the fundamental shield.
- **Your Mission (Production):** Identify and fix a basic XSS flaw.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

**Cross-Site Scripting (XSS)** is a security vulnerability where an attacker injects malicious client-side scripts (usually JavaScript) into a web page viewed by other users. The core of the attack is that the user's browser has no way to know the script is untrusted. The browser sees the script came from a trusted domain (like `yourbank.com`), so it executes it with the full permissions of that site.

**How it Works (The Attack Vector):**

The attack happens when an application takes untrusted data and renders it in the browser without properly neutralizing potentially malicious content. Imagine a blog comment section.

```tsx
// VULNERABLE CODE
const Comment = ({ text }) => {
  // The developer assumes 'text' is just plain text.
  // But an attacker submitted this comment: "<script>stealSessionToken();</script>"
  // This is a classic injection vector.
  return <div dangerouslySetInnerHTML={{ __html: text }} />;
};

```

When another user views this comment, their browser doesn't see a string. It sees a `<div>` and a `<script>` tag. Because the page is from a trusted origin, the browser happily executes `stealSessionToken()`, giving the attacker control over the user's session.

Analogy: XSS is like tricking a trusted news anchor (the website) into reading a malicious message (the script) on live TV. The audience (your browser) trusts the anchor, so they believe and act on the malicious message.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The fundamental defense against XSS is **Context-Aware Output Encoding**. This is a technical way of saying: "Treat all dynamic data as plain text, not executable code." Before rendering any data from an untrusted source, you must "escape" or "encode" special characters so the browser interprets them literally.

**Secure Code Implementation:**

Thankfully, modern frameworks like React do this for you by default. This is one of JSX's most powerful built-in security features.

```tsx
// SECURE CODE
const Comment = ({ text }) => {
  // SAFE BY DEFAULT: React's JSX renderer automatically encodes the content.
  // The attacker's input: "<script>stealSessionToken();</script>"
  // Will be rendered in the HTML as:
  // "&lt;script&gt;stealSessionToken();&lt;/script&gt;"
  // The browser displays this as a harmless string, it does not execute it.
  return <div>{text}</div>;
};

```

React transforms characters like `<` and `>` into their HTML entity equivalents (`&lt;` and `&gt;`). This defangs the attack, turning executable code back into simple, displayable text.

### 🧠 Real-World Case Study: "Vulnerable vs. Secured Code"

**The Goal:** Displaying a user's custom "Status Message" on their profile.

- **Vulnerable Approach:** A developer uses `dangerouslySetInnerHTML` because they think a user might want to use `<b>` or `<i>` tags for styling.
    - **Result:** An attacker sets their status message to `<img src=x onerror="alert('Your session has been compromised. Please log in again.')" />`. When another user views their profile, the alert fires. A more sophisticated attack could present a fake login form and steal credentials.
- **Secure Approach (The React Way):** The developer renders the status directly: `<span>{user.statusMessage}</span>`.
    - **Result:** React's default encoding takes over. The user's profile will literally display the text `<img src=x ...>`. The attack is visible but completely inert. The malicious script is never executed.

### 🤔 Reflective Questions

1. If React's JSX protects against XSS by default, why is it still the most common frontend vulnerability? What actions might a developer take that bypass this default protection?
2. XSS stands for "Cross-Site Scripting". Why "cross-site"? In what way does the attack cross a security boundary?

### 📚 Further Reading

- **OWASP:** [Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/) - The definitive resource on XSS attacks.
- **React Docs:** [Introducing JSX](https://www.google.com/search?q=https://react.dev/learn/writing-markup-with-jsx%23jsx-prevents-injection-attacks) - Read the section on how JSX prevents injection attacks.

### 📝 Mini Task (Production)

You are given a React component that displays a product name fetched from an API. The product manager wants the product name to be rendered as raw HTML because some names contain special formatting.

```tsx
const ProductTitle = ({ nameHTML }) => {
  // TODO: This component is vulnerable to XSS.
  return <h1 dangerouslySetInnerHTML={{ __html: nameHTML }} />;
};

```

Your task: Explain in one sentence why this is dangerous. Then, rewrite the component to be secure, assuming you cannot use `dangerouslySetInnerHTML` at all.
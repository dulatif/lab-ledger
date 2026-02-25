💡 CI02.4.3: The Clean-Up Crew: Basic Sanitization Techniques

**Outline:**

- **The Threat (SEEI):** When you must accept complex input (like HTML), attempting to write your own sanitization logic with regex or string replacement is extremely likely to have bypasses.
- **The Defense (PPP):** The principle of "Don't Roll Your Own Crypto." Use a well-known, battle-tested sanitization library that is built on an "allow-list" model.
- **Your Mission (Production):** Configure a sanitization library to safely clean a snippet of user-provided HTML.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **a flawed, homegrown sanitizer**. Writing a parser that can safely handle all the weird edge cases and bypass techniques in HTML is incredibly difficult. Attackers have spent years finding clever ways to hide scripts where simple sanitizers won't find them.

**How it Works (The Attack Vector):**

A developer wants to allow `<b>` tags but remove `<script>` tags in a comment section.

**Vulnerable Code (Simple Regex):**

```ts
// This is a TERRIBLE idea.
const sanitize = (html) => html.replace(/<script.*?>.*?<\\/script>/gi, "");

```

**Bypass Techniques:** An attacker can easily defeat this with dozens of XSS vectors that don't use a `<script>` tag:

- Event Handlers: `<img src="x" onerror="alert('XSS')">`
- Data URIs: `<a href="data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4=">Click Me</a>`
- Non-standard tags: `<body onload=alert('XSS')>`
- Case sensitivity bypass: `<ScRipT>` (though the regex flag `/i` helps here)

Relying on a simple block-list regex is a guaranteed way to get hacked.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"Use a dedicated library that parses HTML into a tree and rebuilds it using an allow-list."** The only safe way to sanitize HTML is to use a library that understands the DOM. It will deconstruct the input, examine every tag and attribute, and then create a new, clean HTML string containing only the elements you have explicitly permitted.

**Secure Code Implementation (Using DOMPurify):**

While `DOMPurify` is a client-side library, the principles are identical for server-side libraries (like `sanitize-html` in the Node.js ecosystem).

**Client-Side Example (`DOMPurify`):** First: `npm install dompurify`

```tsx
// SECURE CODE
import DOMPurify from 'dompurify';

function UserComment({ dirtyHtml }) {
  // 1. Define your allow-list configuration.
  const cleanHtml = DOMPurify.sanitize(dirtyHtml, {
    USE_PROFILES: { html: true }, // Start with common HTML elements
    ALLOWED_TAGS: ['b', 'i', 'a', 'p'], // Whitelist only these tags
    ALLOWED_ATTR: ['href'] // Allow ONLY the href attribute (on any tag)
  });

  // 2. The output is now safe to render.
  return <div dangerouslySetInnerHTML={{ __html: cleanHtml }} />;
}

// --- Usage ---
// Attacker's input:
const maliciousComment = `
  <p>Hello! <a href="javascript:alert('XSS')">Click me</a></p>
  <img src="x" onerror="alert('Hacked!')">
`;

// <UserComment dirtyHtml={maliciousComment} />
// Rendered Output will be:
// <div><p>Hello! <a>Click me</a></p></div>
// The javascript: URL, <img> tag, and onerror attribute are all gone.

```

The library threw away everything it wasn't explicitly told to keep. This is a secure, allow-list approach.

### 🤔 Reflective Questions

1. DOMPurify prevents XSS. If you use it correctly before rendering user content with `dangerouslySetInnerHTML`, have you completely solved the XSS problem? What other security layers (like CSP) are still valuable?
2. Why is it generally better to perform sanitization on the server before storing content in the database, rather than sanitizing it on the client every time it's about to be displayed?

### 📚 Further Reading

- **DOMPurify:** [GitHub Repository](https://github.com/cure53/DOMPurify) - The gold standard for client-side HTML sanitization.
- **sanitize-html (for Node.js):** [NPM Page](https://www.npmjs.com/package/sanitize-html) - A popular and powerful server-side alternative with a similar allow-list API.

### 📝 Mini Task (Production)

You are using a sanitization library that takes a configuration object. You need to configure it to sanitize a user's bio. The rules are:

- Allowed tags are paragraphs (`<p>`), bold (`<b>`), and italic (`<i>`).
- No attributes of any kind are allowed (no `href`, `class`, etc.).

Your task: Write the configuration object that enforces these rules.

```ts
const config = {
  // Your code here
};

```
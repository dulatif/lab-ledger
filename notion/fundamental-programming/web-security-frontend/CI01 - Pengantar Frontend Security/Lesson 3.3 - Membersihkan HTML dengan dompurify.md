💡 CI01.3.3: The Scrubber: Cleaning HTML with DOMPurify

**Outline:**

- **The Threat (SEEI):** The need to render user-generated HTML without a safe way to do so, forcing a choice between a broken UI and a vulnerable one.
- **The Defense (PPP):** Using `DOMPurify`, a trusted, allowlist-based sanitizer, to surgically remove all malicious content from an HTML string.
- **Your Mission (Production):** Implement `DOMPurify` to secure a vulnerable component.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

You've been tasked with rendering content from a rich text editor. You know that `dangerouslySetInnerHTML` is the tool, and you know it's dangerous. The problem is, how do you clean the HTML? Writing your own sanitizer with regular expressions is a classic security anti-pattern. It's impossible to create a blacklist that catches every clever trick an attacker can devise (e.g., using different character encodings, event handlers, malformed tags, etc.). **Attempting to build your own HTML sanitizer is the vulnerability.**

**How it Works (The Attack Vector):**

A well-meaning developer tries to prevent XSS by writing a regex to remove `<script>` tags before rendering user content.

```tsx
// VULNERABLE CODE - A homemade, insecure sanitizer
function simpleSanitize(html) {
  // This is a terrible idea. It's easily bypassable.
  return html.replace(/<script.*?>.*?<\\/script>/gi, '');
}

const UserComment = ({ commentHTML }) => {
  const cleanHTML = simpleSanitize(commentHTML);
  // An attacker's payload: "<img src=x onerror=alert('Still works!')>"
  // The regex does nothing to this, and the script executes.
  return <div dangerouslySetInnerHTML={{ __html: cleanHTML }} />;
};

```

The developer's blacklist is laughably easy to bypass. The attacker doesn't even need a `<script>` tag; they can use any of the dozens of event handler attributes (`onload`, `onerror`, `onfocus`, etc.) to execute their code.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The principle of defense is: **"Don't Roll Your Own Crypto, and Don't Roll Your Own Sanitizer."** For complex security tasks, always use a well-vetted, industry-standard, open-source library. For client-side HTML sanitization, the gold standard is **DOMPurify**. It works by parsing the HTML string into a DOM tree, walking through it, and removing everything that isn't on a pre-defined allowlist of safe tags and attributes. This allowlist approach is infinitely more secure than a blacklist.

**Secure Code Implementation:**

First, you install the library: `npm install dompurify`. Then, you use it to scrub your HTML before rendering.

```tsx
// SECURE CODE - Using a real sanitizer
import DOMPurify from 'dompurify';

const UserComment = ({ commentHTML }) => {
  // STEP 1: Pass the dirty HTML to DOMPurify.
  // DOMPurify will remove the 'onerror' attribute from the <img> tag,
  // because 'onerror' is not on its allowlist of safe attributes.
  // It will keep safe tags like <b>, <i>, <p>, etc.
  const cleanHTML = DOMPurify.sanitize(commentHTML);

  // STEP 2: Now it is safe to render the cleaned HTML.
  return <div dangerouslySetInnerHTML={{ __html: cleanHTML }} />;
};

```

This is the correct, professional pattern for handling user-generated HTML. You delegate the complex, dangerous task of sanitization to a library whose sole purpose is to do it securely.

### 🧠 Real-World Case Study: "Vulnerable vs. Secured Code"

**The Goal:** A product review system that allows users to add formatted reviews.

- **Vulnerable Approach:** The team builds their own simple sanitizer that only removes `<script>` and `<iframe>` tags.
    - **Result:** An attacker posts a review containing the payload: `<a href="javascript:alert('XSS')">Click for a discount!</a>`. The homemade sanitizer doesn't recognize the `javascript:` protocol in the `href` as a threat. When another user clicks the link, the script executes.
- **Secure Approach:** The team uses DOMPurify.
    - **Result:** DOMPurify's default configuration knows that the `javascript:` protocol is a common attack vector. It will either strip the `href` attribute from the `<a>` tag entirely or remove the malicious link, rendering the anchor tag inert and harmless. The feature works, and the attack is prevented.

### 🤔 Reflective Questions

1. DOMPurify operates on an "allowlist" principle. Why is this inherently more secure than a "blacklist" approach for sanitization?
2. DOMPurify is a JavaScript library that runs in the user's browser. What is a potential downside or limitation of performing sanitization _only_ on the client-side? Why might you also want to sanitize on the server-side?

### 📚 Further Reading

- **Library:** [DOMPurify on GitHub](https://github.com/cure53/DOMPurify) - The official repository. A great example of a security-focused library.
- **OWASP:** [XSS Filter Evasion Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html) - A terrifying list of all the clever ways attackers can bypass weak filters, illustrating why you should never write your own.

### 📝 Mini Task (Production)

You are given the vulnerable component from the `dangerouslySetInnerHTML` lesson.

```tsx
// VULNERABLE
const BlogPost = ({ postContentFromAPI }) => {
  return <div dangerouslySetInnerHTML={{ __html: postContentFromAPI }} />;
};

```

Your task: Refactor this component to be secure. Import `DOMPurify` and add the single line of code required to sanitize the `postContentFromAPI` before it is rendered.
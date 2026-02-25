💡 CI01.3.4: The Secure Rendering Checklist: Best Practices

**Outline:**

- **The Threat (SEEI):** Understanding that XSS vulnerabilities can exist beyond just `innerHTML`, lurking in unexpected places like `href` attributes and CSS properties.
- **The Defense (PPP):** A practical checklist for securely handling untrusted data in different rendering contexts.
- **Your Mission (Production):** Conduct a security review of a complex component and identify multiple vulnerabilities.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is a narrow focus. Many developers learn to avoid `dangerouslySetInnerHTML` but assume that's the only way XSS can happen in React. This is false. An attacker can achieve XSS if you unsafely place user-controlled data into other parts of your component, such as URL attributes (`href`), CSS properties (`style`), or when interacting with third-party libraries. You need a holistic view of data rendering.

**How it Works (The Attack Vectors):**

1. **The `javascript:` Protocol in Links:**
    
    ```tsx
    // VULNERABLE CODE
    const UserLink = ({ userSuppliedURL }) => {
      // Attacker provides the URL: "javascript:alert('XSS')"
      // When a user clicks this link, the script executes.
      return <a href={userSuppliedURL}>Visit my homepage</a>;
    };
    
    ```
    
2. **`url()` in Inline Styles:**
    
    ```tsx
    // VULNERABLE CODE (in older browsers)
    const UserAvatar = ({ userImageURL }) => {
      // Attacker provides the URL: "x" onerror="alert('XSS')"
      // Some older browsers might execute script within CSS's url().
      const style = { backgroundImage: `url(${userImageURL})` };
      return <div style={style}></div>;
    };
    
    ```
    

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The principle is to follow a **Secure Rendering Checklist.** Before you render any piece of untrusted data, ask yourself these questions:

1. **Am I rendering into the HTML DOM?**
    
    - **YES:** Use default JSX `{data}`. This is the safest way. If you _must_ render HTML, use `DOMPurify` before passing it to `dangerouslySetInnerHTML`.
2. **Am I rendering into an HTML attribute?**
    
    - **YES:** Be careful. For `href`, `src`, or `action` attributes, you must validate that the URL has a safe protocol (e.g., it starts with `http:`, `https:`, `mailto:`, or `/`). A simple allowlist check is effective here.
        
    - **Example Validation:**
        
        ```tsx
        function isValidURL(url) {
          const safeProtocols = ['https:', 'http:', 'mailto:', '/'];
          try {
            const parsedURL = new URL(url, window.location.origin);
            return safeProtocols.includes(parsedURL.protocol);
          } catch (e) {
            return false; // Invalid URL format
          }
        }
        
        ```
        
3. **Am I rendering into an inline `style` prop?**
    
    - **YES:** Do not pass full, user-controlled CSS strings. Instead, pass an object of key-value pairs where you control the keys. Let React construct the style string safely.
    - **SECURE:** `const style = { color: userSuppliedColor };`
    - **DANGEROUS:** `const style = userSuppliedStyleString;`
4. **Am I passing data to a third-party library (e.g., a charting or mapping library)?**
    
    - **YES:** Read their documentation carefully. Understand if the library expects plain text or HTML. If it has an option to render HTML tooltips or labels, ensure that data is either encoded by the library or sanitized by you first.

### 🧠 Real-World Case Study: "Vulnerable vs. Secured Code"

**The Goal:** A customizable user profile card where users can set a profile link and a theme color.

- **Vulnerable Approach:** The developer directly assigns the user's input to the `href` of a link and the `color` property of a style object without any validation.
    - **Result:** An attacker sets their profile link to `"javascript:alert('Stealing your data!')"` and their theme color to something benign. When an admin views their profile and clicks the link, the script executes. The developer secured against HTML injection but forgot about protocol injection.
- **Secure Approach:** The developer implements a validation function for the URL. Before rendering the link, they check if the URL starts with `http:` or `https:` and display it only if it's valid. The color is rendered safely as it is not a vector in modern browsers when used correctly.
    - **Result:** The attacker's malicious `javascript:` link is detected as invalid. The application can choose to show a broken link icon, a default link, or nothing at all. The XSS attack is prevented.

### 🤔 Reflective Questions

1. Modern React versions are much better at neutralizing XSS vectors in `style` props. Why is it still considered a best practice to validate and control the data passed to them?
2. Imagine you are building a component that renders user-submitted Markdown. What steps would you need to take to do this securely? (Hint: It involves a conversion step and a sanitization step).

### 📚 Further Reading

- **OWASP:** [DOM-based XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html) - Covers many of these "sink" locations where data can be unsafely rendered.

### 📝 Mini Task (Production)

You are conducting a security code review on the following React component.

```tsx
const UserCard = ({ user }) => {
  // user object from API:
  // {
  //   name: "Malory",
  //   website: "javascript:alert('XSS from website link')",
  //   bio: "An attacker's bio.<script>runEvilStuff()</script>",
  //   profileTheme: { cardStyle: "background: blue;" } // This is just an example
  // }

  return (
    <div style={user.profileTheme.cardStyle}>
      <h1>{user.name}</h1>
      <a href={user.website}>Visit my site</a>
      <p dangerouslySetInnerHTML={{ __html: user.bio }}></p>
    </div>
  );
};

```

Your task: Identify the **three distinct XSS vulnerabilities** in this component and briefly describe how you would fix each one.
💡 AE01-5.1: Secure by Default Components

**Outline:**

- **The Threat (SEEI):** Components with "insecure" props (like those accepting raw HTML or using `dangerouslySetInnerHTML`) create APIs where the easiest path for a developer is the most dangerous one.
- **The Defense (PPP):** The principle of "Paving the Secure Path." Design component APIs where the safest way to use them is also the simplest and most obvious way. Make insecurity an explicit, opt-in choice.
- **Your Mission (Production):** Refactor a dangerous `AlertBox` component to make it secure by default.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **Insecure Component APIs.** A component is "insecure by default" if its props encourage or allow the direct injection of unsanitized content. The most infamous example is React's own `dangerouslySetInnerHTML`. Any component that exposes a similar prop, like `<Component htmlContent={userInput} />`, creates a security footgun. It puts the burden on every single developer who uses that component to remember to sanitize the input every single time. A single mistake leads to an XSS vulnerability.
    
- **How it Works (The Attack Vector):** A team builds a reusable `UserProfile` component. To allow for rich text bios, they add a prop that accepts an HTML string.
    
    ```ts
    // VULNERABLE COMPONENT DESIGN
    const UserProfile = ({ htmlBio }) => {
      // The component's API invites disaster.
      return <div dangerouslySetInnerHTML={{ __html: htmlBio }} />;
    };
    
    // A developer, rushing to finish a feature, uses it incorrectly.
    const user = await fetchUserFromApi(); // user.bio might be "<script>alert('xss')</script>"
    <UserProfile htmlBio={user.bio} />; // Vulnerability created!
    
    ```
    
    The problem isn't just the final line; it's the design of `UserProfile`. Its API makes it easy to do the wrong thing.
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Make security the default, and insecurity difficult.** A developer should have to go out of their way and type something that looks intentionally risky (like `dangerously...`) to bypass the safe path.
    
- **Secure Code Implementation (API Design):** Let's fix the `UserProfile` component.
    
    **Option 1: Disallow HTML entirely (Safest)** The default prop should accept only plain text. React will handle the escaping automatically.
    
    ```ts
    // SECURE BY DEFAULT
    const UserProfile = ({ bio }) => {
      // The default usage is now safe.
      return <div>{bio}</div>;
    };
    
    <UserProfile bio={user.bio} />; // This is now safe!
    
    ```
    
    **Option 2: Force Sanitization for HTML** If you absolutely must support HTML, create a separate, explicitly named prop and build the sanitization into the component itself.
    
    ```tsx
    import DOMPurify from 'dompurify';
    
    const UserProfile = ({ bio, dangerouslySetUnsanitizedHtml }) => {
      if (dangerouslySetUnsanitizedHtml) {
        const cleanHtml = DOMPurify.sanitize(dangerouslySetUnsanitizedHtml);
        return <div dangerouslySetInnerHTML={{ __html: cleanHtml }} />;
      }
      return <div>{bio}</div>;
    };
    
    // Safe usage is simple:
    <UserProfile bio={user.bio} />
    
    // Insecure usage is verbose and looks dangerous, prompting caution:
    <UserProfile dangerouslySetUnsanitizedHtml={user.bio} />
    
    ```
    
    This design is better because the component takes responsibility for sanitization, and the prop name serves as a clear warning to the developer.
    

### 🧠 Real-World Case Study: "The Link Component"

- **The Goal:** Create a reusable `<Link>` component that renders an `<a>` tag.
- **The System:**
    - **Insecure Design:** `<Link href={url} text={htmlText} />` where `htmlText` is passed to `dangerouslySetInnerHTML`. This allows XSS in the link's text.
    - **Secure Design:** The component's main prop is `children`. `<Link href={url}>{children}</Link>`. React's JSX nesting is safe by default. If a developer wants to put complex elements inside, they can, but they can't easily inject a raw HTML string.
- **Vulnerable Implementation:**`const Link = ({ href, text }) => <a href={href} dangerouslySetInnerHTML={{ __html: text }} />`
- **Secure Implementation:**`const Link = ({ href, children }) => <a href={href}>{children}</a>`
    - **Usage:** `<Link href="/home">Go <strong>Home</strong></Link>`
    - **Result:** The secure design is more flexible, more idiomatic to React, and inherently safe from XSS through its primary content prop.

### 🤔 Reflective Questions

1. How does this principle of "secure by default" apply to backend API design, not just frontend components?
2. Think about a component that renders an image: `<Image src={url} />`. What kind of validation could you add inside the `Image` component to make it more secure by default (e.g., against `javascript:` URLs)?
3. Why is it better to have the sanitization logic _inside_ the reusable component rather than requiring every developer who uses it to import and run the sanitizer themselves?

### 📚 Further Reading

- **React Docs:** [Dangerously Set Inner HTML](https://www.google.com/search?q=https://react.dev/reference/react-dom/components/common%23dangerouslysetinnerhtml)
- **OWASP:** [Cross-Site Scripting Prevention Cheat Sheet](https://www.google.com/search?q=https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Scripting_Prevention_Cheat_Sheet.html) (The principles apply to component design).
- **Blog Post:** [Writing forward-compatible components](https://www.google.com/search?q=https://react.dev/blog/2024/04/25/writing-forward-compatible-components%23props-should-be-descriptive-and-structured) (Touches on good prop design).

### 📝 Mini-Task (Production)

You are given a simple `AlertBox` component that is insecure by default.

```tsx
// VULNERABLE COMPONENT
// It's too easy for a developer to pass a dangerous string here.
const AlertBox = ({ message }) => {
  return <div className="alert" dangerouslySetInnerHTML={{ __html: message }} />;
};

// Example of vulnerable usage:
// const urlParams = new URLSearchParams(window.location.search);
// const status = urlParams.get('status'); // e.g., "Success!<script>alert(1)</script>"
// <AlertBox message={status} />

```

Your mission:

1. Refactor the `AlertBox` component.
2. The default, simple prop `message` should now only render plain text and be completely safe.
3. Add a new, dangerously-named prop that allows for HTML, but ensure the component sanitizes this HTML using a library like `DOMPurify` before rendering.
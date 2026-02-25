💡 AE01-5.3: Type-Safe Security with TypeScript

**Outline:**

- **The Threat (SEEI):** In vanilla JavaScript, a `string` is just a string. There is no way to differentiate between a raw, untrusted user input string and a string that has been vetted or sanitized. This ambiguity makes it easy to pass dangerous data to a sensitive function.
- **The Defense (PPP):** Using TypeScript's powerful type system to create "branded" or "opaque" types. We can create types like `SanitizedHTML` or `SafeURL` that can _only_ be created through a specific, secure factory function, preventing accidental misuse at compile time.
- **Your Mission (Production):** Time to build a `SafeURL` type and a component that can only accept this trusted type for its `href` prop.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **Type Ambiguity.** Consider a function `createLink(url: string)`. The `string` type gives us no information about the provenance of the data. Is it a hardcoded internal link like `/about`? Is it a user-submitted URL from a profile page? Could it be a malicious `javascript:alert('XSS')` string? Without a stronger type system, the only way to know is to trace the data flow, which is unreliable in large applications. This ambiguity is a breeding ground for security bugs.
    
- **How it Works (The Attack Vector):** A developer writes a component to display a user's website link.
    
    ```tsx
    // The type signature is too permissive.
    const UserLink = ({ url }: { url: string }) => {
      // A malicious user could have `javascript:alert('pwned')` as their URL.
      return <a href={url}>Visit my site</a>;
    };
    
    // Elsewhere, another developer uses this component with data from an API.
    const userData = await fetchUser(); // userData.websiteUrl is a raw string
    <UserLink url={userData.websiteUrl} /> // A vulnerability is created.
    
    ```
    
    The problem is that the `UserLink` component's contract (`url: string`) implicitly trusts any string it receives. The TypeScript compiler sees `string` and allows it, having no concept of "safe" vs "unsafe" strings.
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Encode security properties into the type system.** Use the compiler as your first line of defense to make invalid states or data flows impossible to represent.
    
- **Secure Code Implementation (Branded Types):** We can create a special "branded" type. It's structurally a string, but with an extra piece of type information that makes it unique.
    
    ```tsx
    import DOMPurify from 'dompurify';
    
    // 1. Define the branded type. It's a string AND an object with a unique brand.
    type SanitizedHTML = string & { readonly __brand: 'SanitizedHTML' };
    
    // 2. Create a "factory" function. This is the ONLY way to create a SanitizedHTML type.
    function createSanitizedHTML(rawHtml: string): SanitizedHTML {
      // This function contains our security logic.
      const sanitized = DOMPurify.sanitize(rawHtml);
      return sanitized as SanitizedHTML; // We assert that the output is now safe.
    }
    
    // 3. Create a component that REQUIRES the safe type.
    const SafeHtmlRenderer = ({ content }: { content: SanitizedHTML }) => {
      return <div dangerouslySetInnerHTML={{ __html: content }} />;
    };
    
    // 4. Usage example:
    const untrustedInput = "<img src=x onerror=alert('XSS')>";
    
    // COMPILE ERROR! Type 'string' is not assignable to type 'SanitizedHTML'.
    // <SafeHtmlRenderer content={untrustedInput} />
    
    // THIS WORKS! We are forced to go through the secure factory function.
    const trustedInput = createSanitizedHTML(untrustedInput);
    <SafeHtmlRenderer content={trustedInput} />;
    
    ```
    
    This pattern makes the secure path the only one the compiler will allow.
    

### 🧠 Real-World Case Study: "Preventing SQL Injection with Types"

- **The Goal:** Prevent SQL injection vulnerabilities at the type level on the server-side (the principle is identical).
- **The System:** A backend data access layer written in TypeScript.
- **Vulnerable Approach:**`function getUsers(orderBy: string) { database.query(`SELECT * FROM users ORDER BY ${orderBy}`) }` The `orderBy` parameter is a raw string. An attacker can pass `id; DROP TABLE users;` and cause a disaster.
- **Secure Approach (With Branded Types):**
    
    1. Define a type `SafeSQLOrderBy = string & { __brand: 'SafeSQLOrderBy' }`.
    2. Create a factory function `createOrderBy(field: string): SafeSQLOrderBy`. This function checks the `field` string against a whitelist of allowed column names (e.g., `['id', 'name', 'createdAt']`). If it's on the list, it returns the branded type. If not, it throws an error.
    3. Change the function signature to `getUsers(orderBy: SafeSQLOrderBy)`.
    
    - **Result:** It is now a compile-time error to call `getUsers` with a raw user input string. A developer is forced to validate the input using `createOrderBy` first, completely eliminating this vector of SQL injection.

### 🤔 Reflective Questions

1. Branded types are a powerful pattern. What are the potential downsides? (e.g., boilerplate, complexity for new developers).
2. How could you use this same pattern to ensure that a function that processes sensitive data has been called _after_ a user has been properly authenticated? (Hint: The function could require an `AuthenticatedUser` object that can only be created by your auth logic).
3. This pattern moves security checks from "runtime" to "compile time." Why is this a massive advantage for building secure and reliable systems?

### 📚 Further Reading

- **TypeScript Deep Dive:** [Branded Types](https://www.google.com/search?q=https://basarat.gitbook.io/typescript/main-1/nominal-typing)
- **Blog Post by Snyk:** [Type-driven security: Preventing security anti-patterns with TypeScript](https://www.google.com/search?q=https://snyk.io/blog/type-driven-security-preventing-security-anti-patterns-with-typescript/)
- **Matt Pocock's Blog:** [Total TypeScript Opaque Types](https://www.google.com/search?q=https://www.totaltypescript.com/opaque-types)

### 📝 Mini-Task (Production)

You are building a component that displays a user's avatar from a URL. To prevent XSS via `javascript:` URLs, you must ensure that any URL passed to the component has been validated to start with `https://`.

Your mission:

1. Create a branded type named `SafeURL`.
2. Create a factory function `createSafeURL(url: string): SafeURL | null`. This function should check if the URL string starts with `https://`. If it does, return the branded type. If not, return `null`.
3. Create a React component `<Avatar src={...} />` where the `src` prop _must_ be of type `SafeURL`.
4. Show an example of how a developer would correctly use this component, and show why passing a raw, unvalidated string would result in a compile-time error.
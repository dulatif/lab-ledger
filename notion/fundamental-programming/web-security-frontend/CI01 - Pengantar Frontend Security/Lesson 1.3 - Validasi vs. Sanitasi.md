💡 CI01.1.3: Gates and Filters

**Outline:**

- **The Threat (SEEI):** Confusing validation with sanitization, leading to flawed defenses.
- **The Defense (PPP):** Using the right tool for the job: validate on entry, sanitize on exit.
- **Your Mission (Production):** Design a security strategy for a new feature.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is a **conceptual misunderstanding** that leads to a critical logic flaw. Developers often confuse **Input Validation** ("Is this data in the correct format?") with **Output Sanitization/Encoding** ("Is this data safe to display in this context?"). Trying to use one to do the other's job is like using a gate to filter water or a filter to stop a car. They are different tools for different problems.

**How it Works (The Attack Vector):**

A developer wants to prevent XSS in a comment field. They try to "validate" the input by creating a blacklist to block `<script>` tags.

```tsx
// VULNERABLE CODE - A FLAWED ATTEMPT AT VALIDATION
const handleSubmit = (commentText) => {
  // Developer tries to block "script" tags. This is a weak blacklist.
  const scriptRegex = /<script\\b[^>]*>[\\s\\S]*?<\\/script\\b[^>]*>/gi;
  if (scriptRegex.test(commentText)) {
    throw new Error("Scripts are not allowed.");
  }
  // The "validated" comment is then sent to the server.
  // Later, it will be rendered with dangerouslySetInnerHTML.
  saveComment(commentText);
};

// An attacker can easily bypass this with:
// 1. Different capitalization: <SCRIPT>alert(1)</SCRIPT>
// 2. Event handlers: <img src=x onerror=alert(1)>
// 3. Different encodings, etc.

```

Blacklist-based validation for security is almost always a losing battle. The attacker has infinite ways to be creative, while you have to predict every single one.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The principle is: **"Validate on Entry, Sanitize on Exit."**

1. **Input Validation (The Gate):** When data first enters your system (from a user form, API, etc.), check if it conforms to your business rules. Is the email _actually_ an email? Is the age a _positive integer_? Is the country code a _valid ISO code_? If not, **reject it**. This is about data integrity and format.
2. **Output Sanitization/Encoding (The Filter):** When you are about to render data, make it safe for the specific context where it will be used. For displaying data in HTML, you encode it so the browser treats it as text. If you must allow some HTML, you sanitize it to remove anything dangerous. This is about preventing injection attacks.

**Secure Code Implementation:**

Let's fix the comment feature using the correct approach.

```tsx
// SECURE CODE - STEP 1: VALIDATION (ON ENTRY)
import { z } from 'zod';

const commentSchema = z.string().min(1, "Comment cannot be empty.").max(500);

const handleSaveComment = (commentText) => {
  // 1. VALIDATE: Check if the comment meets our rules (not empty, not too long).
  // This happens BEFORE we even send it to the server.
  commentSchema.parse(commentText); // Throws an error if invalid
  api.saveComment(commentText);
};

// SECURE CODE - STEP 2: SANITIZATION (ON EXIT/RENDERING)
import DOMPurify from 'dompurify';

// The component that RENDERS comments fetched from the server.
const Comment = ({ content }) => {
  // 2. SANITIZE: We need to render user-provided HTML, so we must clean it.
  // DOMPurify will strip out all dangerous elements and attributes (like <script> or onerror).
  const cleanContent = DOMPurify.sanitize(content);

  // We can now safely use the cleaned HTML.
  return <div dangerouslySetInnerHTML={{ __html: cleanContent }} />;
};

```

### 🧠 Real-World Case Study: "Vulnerable vs. Secured Code"

**The Goal:** A user registration form with a "username" field. The username will be displayed on multiple pages.

- **Vulnerable Approach:** The developer only does input validation. They check if the username is alphanumeric and between 3-15 characters long. They assume this is enough. However, another part of the app fetches this username and renders it without proper output encoding.
    - **Result:** The validation rule was flawed. An attacker signs up with the username `test"><script>alert(1)</script>`. The validation might pass, but when rendered improperly elsewhere, the script executes. The developer relied only on the gate and forgot the filter.
- **Secure Approach:** The developer implements both.
    
    1. **Validation:** Username must be alphanumeric, 3-15 characters.
    2. **Output Encoding:** Everywhere the username is displayed, it's done using standard JSX: `<h1>{username}</h1>`.
    
    - **Result:** The multi-layered defense works. Even if a clever attacker finds a way to bypass the validation rule, the automatic output encoding by React acts as a fail-safe, neutralizing the payload.

### 🤔 Reflective Questions

1. Input validation can happen on the client-side (in React), on the server-side (in your API), or both. What are the security advantages and disadvantages of each location? Why is server-side validation considered non-negotiable?
2. "Output Encoding" (like React's default JSX behavior) is a form of sanitization. How is it different from "HTML Sanitization" performed by a library like `DOMPurify`? In what scenario would you choose one over the other?

### 📚 Further Reading

- **OWASP:** [Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html) - A deep dive into the principles of good validation.
- **Library:** [DOMPurify](https://github.com/cure53/DOMPurify) - The gold standard for client-side HTML sanitization.

### 📝 Mini Task (Production)

You are tasked with building a feature that allows users to create a custom event listing. The user must provide:

1. An `eventName` (string, required, max 100 chars).
2. An `eventDate` (must be a valid future date).
3. A `description` (string, allows some safe HTML like `<b>`, `<i>`, and `<ul><li>` for formatting).

Your task: For each of the three fields, describe your security strategy. Specify whether you would use **validation**, **sanitization**, or **both**, and briefly explain why.
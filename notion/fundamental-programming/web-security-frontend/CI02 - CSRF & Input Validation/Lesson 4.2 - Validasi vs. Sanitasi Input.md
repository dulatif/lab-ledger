💡 CI02.4.2: Gatekeeper vs. Bodyguard: Validation vs. Sanitization

**Outline:**

- **The Threat (SEEI):** A confusion between two distinct security actions: rejecting bad data (validation) and cleaning up potentially harmful data (sanitization). Applying the wrong one can lead to vulnerabilities.
- **The Defense (PPP):** Clearly defining the roles of validation and sanitization and understanding the "Validate First" principle.
- **Your Mission (Production):** For a given input field, decide whether validation or sanitization is the appropriate primary defense.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is a **category error**. Developers often use the terms "validate" and "sanitize" interchangeably, but they are very different actions.

- **Validation** is about enforcing strict rules on the _format_ and _structure_ of data.
- **Sanitization** is about modifying data to _remove_ unwanted or dangerous characters.

Trying to sanitize data that should have been rejected is a common mistake. If you expect a username and get `<script>alert(1)</script>`, your goal isn't to "clean up" the script tag; your goal is to reject the input because it contains characters that are not allowed in a username.

**How it Works (The Attack Vector):**

A developer is creating a "Set Username" feature. The rule is that usernames can only contain alphanumeric characters.

```ts
// VULNERABLE (WRONG APPROACH) CODE
// The developer tries to "sanitize" by removing "<" and ">"
// This is a weak, block-list approach.
const sanitizeUsername = (input) => {
  return input.replace(/</g, "").replace(/>/g, "");
}

// Attacker submits a username: "jane<>" -> server saves "jane" (Maybe okay)
// Attacker submits: "jane; alert(1)" -> server saves the whole thing!
// Attacker submits: "jane onmouseover=alert(1)" -> This could be dangerous in other contexts.

```

The developer should not be trying to _fix_ the bad input. They should be _rejecting_ it.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"Validate, Don't Sanitize, Where Possible."** Your primary defense for input is always validation. Check if the data conforms to a strict, allow-list-based set of rules. If it doesn't, reject it with a `400 Bad Request` error. Only use sanitization as a secondary step when you must accept data that could contain mixed content, like HTML in a blog comment.

**The Roles Defined:**

- **Validation (The Gatekeeper):**
    - **Job:** Checks the ID of everyone trying to enter.
    - **Action:** If the ID is invalid, it denies entry. It does not try to fix the ID.
    - **Example:** Is this input a valid email? Is this input a number between 1 and 10? Is this username only alphanumeric characters?
- **Sanitization (The Bodyguard):**
    - **Job:** Checks the bag of someone who has already been allowed in.
    - **Action:** Removes any prohibited items from the bag before letting them proceed.
    - **Example:** This blog comment is allowed to have `<b>` and `<i>` tags, but I will remove any `<script>` tags and `onclick` attributes.

**Secure Code Implementation (Validation First):**

Let's fix the username example using validation.

```ts
// SECURE CODE (VALIDATION FIRST)
import { z } from 'zod';

const usernameSchema = z.string().regex(/^[a-zA-Z0-9]+$/, {
  message: "Username can only contain letters and numbers"
});

// On the server...
try {
  const validatedUsername = usernameSchema.parse(req.body.username);
  // If we get here, the input is safe and valid.
  saveUsername(validatedUsername);
} catch (error) {
  // If the regex fails, Zod throws, and we reject the request.
  res.status(400).send("Invalid username.");
}

```

This is far more robust. We are only allowing known-good characters.

### 🤔 Reflective Questions

1. Consider a rich text editor that allows users to submit blog posts with formatting. Why is this a classic case where you need both validation (e.g., post must not be empty) and sanitization (e.g., remove dangerous HTML)?
2. "Allow-listing" (defining what is allowed) is a core principle of good validation. "Block-listing" (defining what is forbidden) is often associated with sanitization. Why is allow-listing generally considered more secure?

### 📚 Further Reading

- **OWASP:** [Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html) - This guide is primarily about validation, which underscores its importance as the first line of defense.

### 📝 Mini Task (Production)

You are building a form with a field for a user to enter their desired "discount code." The codes are always 6-digit numbers (e.g., `123456`). A user enters the value `"123-456"`.

Should your backend code's primary response be to **validate** this input (and reject it) or to **sanitize** it (e.g., by stripping the hyphen to get "123456")? Explain your choice in one sentence.
💡 CI02.4.1: The Final Authority: Never Trust the Client

**Outline:**

- **The Threat (SEEI):** A false sense of security, believing that client-side validation is a sufficient security control, when it can be trivially bypassed.
- **The Defense (PPP):** The principle of the "Authoritative Server." Treating all data arriving from the client as untrusted and re-validating it on the server without exception.
- **Your Mission (Production):** Articulate why server-side validation is the only security boundary that matters for input.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **misplaced trust**. It's the critical mistake of assuming that because you have a beautiful React form with robust Zod validation, the data arriving at your server will adhere to those rules. This is fundamentally wrong. Client-side validation is a suggestion; server-side validation is the law. An attacker does not need to use your web interface.

**How it Works (The Attack Vector):**

An attacker can completely bypass all your client-side JavaScript, HTML attributes, and CSS. They can craft a raw HTTP request and send it directly to your API endpoint.

**Bypass Method 1: Browser Dev Tools** A user can open the browser's developer tools, find a disabled "Admin" checkbox in the HTML, remove the `disabled` attribute, check the box, and submit the form.

**Bypass Method 2: API Client (Postman, curl)** An attacker can observe your API's traffic and then craft a malicious request from scratch, sending whatever data they want.

```bash
# An attacker sends a negative quantity, bypassing all UI logic.
curl -X POST <https://your-app.com/api/orders> \\
  -H "Content-Type: application/json" \\
  -H "Cookie: session_id=abc123xyz" \\
  -d '{ "product_id": 123, "quantity": -100 }'

```

If your server trusts the `quantity` value from the client without re-validating it, you now have a serious data integrity problem.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"The client is a hostile environment; the server is the single source of truth."** Client-side validation is for UX. Server-side validation is for security and data integrity. You must re-validate 100% of the input on the server using the same (or stricter) rules you used on the client.

**Secure Code Implementation (Conceptual Server-Side Check):**

This is why having a shared schema is so powerful. You can use the same Zod schema on both the client and the server.

```ts
// SECURE SERVER CODE (Express.js)
import { registrationSchema } from './schemas'; // The same schema used on the frontend!

app.post('/api/register', (req, res) => {
  try {
    // 1. Re-validate the entire request body against the schema.
    const validatedData = registrationSchema.parse(req.body);

    // 2. Only use the validated data from this point forward.
    createUser(validatedData.email, validatedData.password);

    res.status(201).send('User created');
  } catch (error) {
    // 3. If validation fails, reject the request.
    res.status(400).json({ message: "Invalid data", errors: error.errors });
  }
});

```

This endpoint is secure. It doesn't matter what the client sends; if it doesn't conform to the `registrationSchema`, it is rejected.

### 🧠 Real-World Case Study: "The Price is Wrong"

**The Goal:** A user adds an item to their shopping cart and proceeds to checkout.

- **Vulnerable Approach:** The form on the checkout page includes a hidden input with the item's price: `<input type="hidden" name="price" value="99.99">`. When the form is submitted, the server trusts this price and charges the user.
    - **Result:** An attacker uses the browser's developer tools to change the `value` to `"0.01"` before clicking "Purchase". The server receives the request and happily processes a $99.99 order for one cent.
- **Secure Approach:** The form only submits the `product_id`. When the server receives the request, it **ignores any price information sent from the client**. It uses the `product_id` to look up the item's authoritative price in its own database and uses that price to create the charge.

### 🤔 Reflective Questions

1. If you have a Zod schema that you use on both the client and the server, which location is more important to keep up-to-date if a validation rule changes? Why?
2. Thinking about the "hostile environment" concept, what are some other things on the client that a user can manipulate besides form data (e.g., cookies, local storage)?

### 📚 Further Reading

- **OWASP:** [Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html) - Note the emphasis on canonicalization and server-side validation.

### 📝 Mini Task (Production)

Your application has a form that allows a user to update their role. The form submits a `userId` and a `role`. A regular user should only be able to set the role to "viewer". An admin can set the role to "editor" or "admin".

Your task: The client-side logic correctly hides the "editor" and "admin" options for regular users. Explain in one sentence what server-side check is still absolutely necessary to prevent a privilege escalation attack.
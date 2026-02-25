💡 CI02.3.2: The Single Source of Truth: Schema-Based Validation with Zod

**Outline:**

- **The Threat (SEEI):** Validation logic that is scattered, duplicated, and inconsistent. Manually writing `if/else` statements for form validation is fragile and hard to maintain.
- **The Defense (PPP):** The principle of "Declarative Validation." Using a schema library like Zod to create a single, reliable source of truth for the shape of your data.
- **Your Mission (Production):** Create a Zod schema for a user profile object.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **imperative and scattered logic**. As forms get more complex, so does the validation. A developer might write validation checks inside a component's event handler.

```ts
// VULNERABLE (HARD TO MAINTAIN) CODE
const handleSubmit = (event) => {
  const email = event.target.email.value;
  const password = event.target.password.value;

  if (!email.includes('@')) {
    // set error state for email
    return;
  }
  if (password.length < 8) {
    // set error state for password
    return;
  }
  if (!/[0-9]/.test(password)) {
    // set error state for password again
    return;
  }
  // ... and so on
};

```

This code is difficult to read, test, and reuse. If you have another form with an email field, you have to copy-paste this logic. If the password rules change, you have to find and update this logic everywhere it's used. This is a recipe for bugs and inconsistent validation.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

The principle is: **"Define the shape of your data, don't write the steps to check it."** A schema is a declarative definition of what your data should look like. A library like Zod allows you to build this definition and then use it to "parse" (validate) your data objects. This schema becomes the single source of truth for your data's validity.

**Secure Code Implementation (Using Zod):**

First, install Zod: `npm install zod`

Now, let's define a schema for our registration form. This object lives outside our components and can be imported anywhere.

```ts
// SECURE (ROBUST & REUSABLE) CODE
import { z } from 'zod';

// This is the single source of truth for what a valid user registration looks like.
export const registrationSchema = z.object({
  email: z.string().email({ message: "Invalid email address" }),

  password: z.string()
    .min(8, { message: "Password must be at least 8 characters long" })
    .regex(/[A-Z]/, { message: "Password must contain an uppercase letter" })
    .regex(/[0-9]/, { message: "Password must contain a number" }),

  confirmPassword: z.string(),
}).refine(data => data.password === data.confirmPassword, {
  message: "Passwords do not match",
  path: ["confirmPassword"], // Where to show the error
});

// Now, we can use it to validate data.
const userData = {
  email: 'test@example.com',
  password: 'Password123',
  confirmPassword: 'Password123'
};

try {
  const validatedData = registrationSchema.parse(userData);
  console.log("Validation successful!", validatedData);
} catch (error) {
  // If validation fails, Zod throws an error.
  console.error("Validation failed:", error.errors);
}

```

This approach is clean, readable, reusable, and all the logic is co-located.

### 🤔 Reflective Questions

1. One of the biggest advantages of a schema is that it can be used on both the client and the server. How does this help prevent logic duplication and ensure consistent validation across your entire application?
2. Zod is "TypeScript-first." What does this mean? How can you use a Zod schema to automatically infer a TypeScript `type` for your data? (e.g., `type RegistrationData = z.infer<typeof registrationSchema>;`)

### 📚 Further Reading

- **Zod Documentation:** [Zod](https://zod.dev/) - The official documentation is excellent and full of examples.
- **Concept:** [Declarative vs. Imperative Programming](https://ui.dev/imperative-vs-declarative-programming) - Understanding this concept helps clarify why schema-based validation is so powerful.

### 📝 Mini Task (Production)

You need to create a form for a user to update their profile. The data object will have the following fields and rules:

- `username`: a string that must be at least 3 characters long.
- `bio`: a string that is optional and can have a maximum length of 160 characters.
- `websiteUrl`: a string that must be a valid URL and is also optional.

Your task: Write a complete Zod schema named `profileSchema` that defines these validation rules.
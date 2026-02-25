💡 CI02.3.4: Clear and Actionable: Providing Good Error Feedback

**Outline:**

- **The Threat (SEEI):** A form that fails validation but provides no clear, specific, or actionable feedback to the user, leading to frustration and abandonment.
- **The Defense (PPP):** The principle of "Guide, Don't Blame." Using the state from our form library to display helpful, inline error messages that tell the user exactly what is wrong and how to fix it.
- **Your Mission (Production):** Add user-friendly, inline error messages to an existing React Hook Form component.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is a **dead-end user experience**. A user fills out your form, makes a mistake, and clicks submit. The form either does nothing or displays a single, generic error message at the top, like "Invalid input." The user has no idea _which_ field is wrong or _why_ it's wrong. Is their email format incorrect? Is their password too short? This ambiguity is frustrating and makes users feel like they are being blamed for not knowing the rules.

**How it Works (The Attack Vector):**

A user interacts with a form that has validation but poor feedback.

```tsx
// VULNERABLE (POOR UX) CODE
function BadFeedbackForm() {
  const { register, handleSubmit, formState: { isValid } } = useForm({
    // ... resolver is configured
  });

  const onSubmit = data => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* If the form is invalid, nothing tells the user why. */}
      {!isValid && <p>There are errors on the form.</p>}

      <label>Email</label>
      <input {...register('email')} />

      <label>Password</label>
      <input type="password" {...register('password')} />

      <button type="submit">Register</button>
    </form>
  );
}

```

The user is left guessing what the problem is. They might try re-submitting, re-typing everything, or simply giving up and leaving your site.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

Your error feedback must be **Specific, Inline, and Timely.**

- **Specific:** Use the exact error message from your Zod schema. "Password must be at least 8 characters" is infinitely better than "Invalid password."
- **Inline:** Display the error message directly below or next to the field it relates to.
- **Timely:** Show the error after the user has finished interacting with a field (on `blur`) or after they have attempted to submit the form. React Hook Form handles this for you.

**Secure Code Implementation (Displaying Errors from RHF):**

We can expand on our form from the last lesson. The `formState.errors` object provided by `useForm` contains everything we need.

```tsx
// SECURE (GOOD UX) CODE
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { registrationSchema } from './schemas'; // Assuming schema is in another file

function GoodFeedbackForm() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm({
    resolver: zodResolver(registrationSchema),
    // This mode shows errors after the user blurs an invalid field
    mode: 'onBlur',
  });

  const onSubmit = (data) => console.log('Valid data:', data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div>
        <label htmlFor="email">Email</label>
        <input id="email" type="email" {...register('email')} />
        {/* Conditionally render the error message for the email field */}
        {errors.email && (
          <p style={{ color: 'red' }}>{errors.email.message}</p>
        )}
      </div>

      <div>
        <label htmlFor="password">Password</label>
        <input id="password" type="password" {...register('password')} />
        {/* The message comes directly from our Zod schema! */}
        {errors.password && (
          <p style={{ color: 'red' }}>{errors.password.message}</p>
        )}
      </div>

      <button type="submit">Register</button>
    </form>
  );
}

```

Now, if a user types a short password and clicks away, a specific, helpful error message will appear right under the input, guiding them to fix it.

### 🤔 Reflective Questions

1. We set `mode: 'onBlur'` in our `useForm` configuration. What are other validation modes available in React Hook Form (e.g., `onChange`, `onSubmit`), and in what scenarios might you choose one over the other?
2. Beyond just showing a text message, what are some other ways you can visually indicate that a form field has an error (e.g., using CSS)? How does this improve accessibility?

### 📚 Further Reading

- **React Hook Form:** [`formState`](https://react-hook-form.com/docs/useform/formstate) - The official documentation for the `formState` object, including `errors`, `isDirty`, `isValid`, etc.
- **Nielsen Norman Group:** [Error Message Guidelines](https://www.nngroup.com/articles/error-message-guidelines/) - Foundational UX principles for writing effective error messages.

### 📝 Mini Task (Production)

You are working on a React Hook Form component. The `errors` object from `formState` is available. You have a field that was registered with the name `username`.

Your task: Write the JSX snippet that will conditionally render the error message for the `username` field inside a `<span>` tag.
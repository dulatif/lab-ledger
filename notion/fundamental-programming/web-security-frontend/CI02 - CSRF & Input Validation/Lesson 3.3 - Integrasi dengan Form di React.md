💡 CI02.3.3: The Orchestrator: Integrating with React Hook Form

**Outline:**

- **The Threat (SEEI):** Manually managing form state (`useState` for every field), handling user input (`onChange`), tracking errors, and triggering validation is a huge amount of boilerplate code that is prone to bugs and performance issues.
- **The Defense (PPP):** The principle of "Controlled Uncontrol." Using a dedicated form library like React Hook Form to orchestrate form state and validation, keeping your components clean and performant.
- **Your Mission (Production):** Refactor a basic form component to use React Hook Form with a Zod schema.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **complexity and poor performance**. A "simple" registration form in React with manual state management quickly becomes a mess.

```tsx
// VULNERABLE (COMPLEX & UNPERFORMANT) CODE
function ManualForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [errors, setErrors] = useState({});

  const handleSubmit = (e) => {
    // Manually run validation...
    // Manually set errors...
  };

  // Every keystroke in any input causes the entire component to re-render.
  // This is a major performance problem for large forms.
  return (
    <form onSubmit={handleSubmit}>
      <input value={email} onChange={e => setEmail(e.target.value)} />
      {/* ... and so on for every field ... */}
    </form>
  );
}

```

This approach has several problems: managing many `useState` hooks, excessive re-renders on every keystroke, and manually wiring up our Zod schema to this state.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"Don't reinvent the form; let a library handle the boilerplate."** React Hook Form is a library designed specifically to solve these problems. It manages form state efficiently (often without re-renders) and provides a seamless way to connect your validation schema (like Zod) to your form UI.

**Secure Code Implementation (React Hook Form + Zod):**

First, install the required packages: `npm install react-hook-form @hookform/resolvers zod`

Now, we can create a clean, performant, and robust form.

```tsx
// SECURE (CLEAN & PERFORMANT) CODE
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

// We import our schema from the previous lesson
const registrationSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
});

// Infer the TypeScript type from the schema
type RegistrationFormData = z.infer<typeof registrationSchema>;

function RegistrationForm() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm<RegistrationFormData>({
    // Tell React Hook Form to use our Zod schema for validation
    resolver: zodResolver(registrationSchema),
  });

  // This function only runs if validation passes
  const onSubmit = (data: RegistrationFormData) => {
    console.log('Form data is valid!', data);
    // Now you can send `data` to your API
  };

  return (
    // handleSubmit will process form data and call our onSubmit function
    <form onSubmit={handleSubmit(onSubmit)}>
      <div>
        <label htmlFor="email">Email</label>
        {/* `register` connects the input to the form state */}
        <input id="email" type="email" {...register('email')} />
        {/* We'll cover displaying errors in the next lesson */}
        {errors.email && <p>{errors.email.message}</p>}
      </div>

      <div>
        <label htmlFor="password">Password</label>
        <input id="password" type="password" {...register('password')} />
        {errors.password && <p>{errors.password.message}</p>}
      </div>

      <button type="submit">Register</button>
    </form>
  );
}

```

This component is much cleaner. React Hook Form handles state, `onChange`, and running validation. We just define the schema and tell the library to use it.

### 🤔 Reflective Questions

1. React Hook Form is based on uncontrolled components, which is why it's so performant. What is the difference between a controlled and an uncontrolled input in React?
2. The `zodResolver` is the glue between Zod and React Hook Form. What do you think a "resolver" is doing under the hood? What is its main job?

### 📚 Further Reading

- **React Hook Form:** [Get Started](https://react-hook-form.com/get-started) - The official documentation is the best place to learn.
- **@hookform/resolvers:** [Documentation](https://github.com/react-hook-form/resolvers) - See how to integrate with other validation libraries like Yup, Joi, etc.

### 📝 Mini Task (Production)

You have a Zod schema called `loginSchema` for a login form, which validates `email` and `password` fields.

Your task: In a React component, write the single line of code that calls the `useForm` hook and configures it to use `zodResolver` with your `loginSchema`.
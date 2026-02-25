# 💡 AT01.5.4: The User's Journey: Integration Testing with React Testing Library

**Outline:**

- **Beyond the Unit (Presentation):** Understanding why 100% unit test coverage isn't enough and the role of integration tests.
- **Testing Like a User (Practice):** An introduction to React Testing Library and its philosophy of testing from the user's perspective.
- **Testing a Full Workflow (Production):** Writing a test that simulates a complete user interaction with your PWA.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Beyond the Unit**

We've successfully tested our Redux reducers in isolation. This is fantastic, but it only proves that the individual gears of our machine work correctly. It doesn't prove that the gears fit together to make the machine run.

That's the job of **integration testing**. An integration test verifies that multiple components and services work together as expected to fulfill a user workflow. For example:

- Can a user type in a form, click a button, and see the correct data submitted to the Redux store?
- If the Redux store updates, does the UI re-render correctly to reflect that change?

A unit test can't answer these questions. An integration test can. For this, our tool of choice is **React Testing Library (RTL)**. Its philosophy is simple but powerful: write tests that resemble how a real user interacts with your app. You don't test implementation details; you test the end result that the user experiences.

### **Part 2: Practice - Testing Like a User**

RTL encourages you to interact with your components through the eyes (and keyboard and mouse) of a user.

- **Instead of:** Finding a component by its instance or class name (`wrapper.find(MyButton)`).
- **You do:** Find an element by what the user sees: its accessible role (`screen.getByRole('button', { name: /submit/i })`), its label text (`screen.getByLabelText(/username/i)`), or its placeholder text.

Let's test a login form.

**The Test (using Vitest/Jest and RTL):**

```tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { describe, it, expect, vi } from 'vitest';
import { LoginForm } from './LoginForm';
import { Provider } from 'react-redux';
import { createTestStore } from '../test-utils'; // A helper to create a store for testing

describe('LoginForm', () => {
  it('should allow a user to log in', async () => {
    const store = createTestStore();
    render(
      <Provider store={store}>
        <LoginForm />
      </Provider>
    );

    const user = userEvent.setup();

    // 1. Find the elements like a user would
    const emailInput = screen.getByLabelText(/email/i);
    const passwordInput = screen.getByLabelText(/password/i);
    const submitButton = screen.getByRole('button', { name: /log in/i });

    // 2. Interact with the form
    await user.type(emailInput, 'test@example.com');
    await user.type(passwordInput, 'password123');
    await user.click(submitButton);

    // 3. Assert the result
    // We expect a "login" action to have been dispatched to our mock store
    const actions = store.getActions();
    expect(actions[0].type).toBe('auth/login/pending');
    expect(actions[0].payload).toEqual({
      email: 'test@example.com',
      password: 'password123',
    });
  });
});
```

This test provides extremely high confidence. It proves that the form renders, the user can interact with it, and clicking the button dispatches the correct action to our Redux store.

## 🧠 **Real-World Case Study: "The Silent Failure"**

- **The Bug:** A PWA had a form for creating a new task. The `TextField` component and the `SubmitButton` component both had 100% unit test coverage. The Redux `tasksSlice` reducer also had 100% unit test coverage. However, in production, the form did nothing when submitted.
- **The Cause:** A developer had accidentally changed the `type` prop on the `SubmitButton` from `"submit"` to `"button"`. The button still rendered and could be clicked, but it no longer triggered the form's `onSubmit` event. None of the unit tests caught this because they never tested the two components _together_.
- **The Fix:** The team wrote a single integration test using React Testing Library that rendered the entire form. The test simulated typing into the text field and clicking the button. It then asserted that a `createTask` action was dispatched. The test failed immediately, revealing the incorrect button `type`.
- **The Lesson:** Integration tests are essential for verifying the "wiring" between your components and services. They catch the exact type of bugs that slip through the cracks of even perfect unit test coverage.

## 🤔 **Reflective Questions**

1. Why is it better to query by `role` or `labelText` instead of `data-testid`?
2. What's the difference between `getBy...`, `findBy...`, and `queryBy...` queries in RTL?
3. How can you provide a mock Redux store or mock context providers to your components during testing?

## 📚 **Further Reading**

- **React Testing Library Docs:** [Official Documentation](https://testing-library.com/docs/react-testing-library/intro/)
- **Kent C. Dodds:** [Testing Implementation Details](https://kentcdodds.com/blog/testing-implementation-details) (The core philosophy behind RTL).

## 📝 **Mini Task (Production)**

Write an integration test for the "add todo" functionality in your PWA.

1. Render your main `App` component wrapped in a test Redux provider.
2. Use `screen.getByPlaceholderText(...)` to find the input for a new todo.
3. Use `userEvent.type(...)` to simulate the user typing "Buy milk".
4. Use `screen.getByRole('button', ...)` to find the "Add Todo" button and `userEvent.click(...)` to click it.
5. Use `screen.findByText('Buy milk')` to assert that the new todo item has appeared in the document. This proves the entire workflow—from UI interaction to Redux dispatch to UI re-render—is working correctly.
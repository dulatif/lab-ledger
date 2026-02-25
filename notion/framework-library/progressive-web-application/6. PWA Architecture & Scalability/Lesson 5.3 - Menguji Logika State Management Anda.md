# 💡 AT01.5.3: Bulletproofing the Brain: Testing Your Redux Logic

**Outline:**

- **The Beauty of Pure Functions (Presentation):** Why Redux reducers are the easiest part of any application to test.
- **Testing a Reducer (Practice):** A step-by-step guide to writing unit tests for a Redux slice with Vitest/Jest.
- **Testing a Thunk (Production):** Writing tests for your asynchronous state logic.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Beauty of Pure Functions**

One of the core principles of Redux is that your reducers must be **pure functions**. This means:

- Given the same inputs (the current `state` and an `action`), they will always return the exact same output (the `nextState`).
- They have no side effects (they don't modify the original state, make API calls, or touch global variables).

This constraint isn't just for architectural purity; it's a huge gift when it comes to testing. Testing a pure function is trivial. You call it with some input, and you assert that the output is what you expect. There's no need to mock dependencies, deal with async behavior, or render components.

Because the core logic of your application's state transitions lives in these simple, pure functions, you can write fast, reliable, and comprehensive unit tests for them. This allows you to build a rock-solid foundation for your PWA with a high degree of confidence.

### **Part 2: Practice - Testing a Reducer**

Let's test a simple `counterSlice`.

**The Slice:**

```ts
// features/counter/counterSlice.ts
import { createSlice } from '@reduxjs/toolkit';

const initialState = { value: 0 };

const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    reset: () => initialState,
  },
});

export const { increment, reset } = counterSlice.actions;
export default counterSlice.reducer;
```

**The Test File (using Vitest/Jest syntax):**

```ts
// features/counter/counterSlice.spec.ts
import { describe, it, expect } from 'vitest';
import counterReducer, { increment, reset } from './counterSlice';

describe('counter reducer', () => {
  it('should handle initial state', () => {
    // Call the reducer with undefined state to check initial state
    expect(counterReducer(undefined, { type: 'unknown' })).toEqual({ value: 0 });
  });

  it('should handle increment', () => {
    const initialState = { value: 5 };
    const nextState = counterReducer(initialState, increment());
    expect(nextState.value).toBe(6);
  });

  it('should handle reset', () => {
    const currentState = { value: 100 };
    const nextState = counterReducer(currentState, reset());
    expect(nextState.value).toBe(0);
  });
});
```

It's that simple. We import the reducer and the action creators, call the reducer with a starting state and an action, and check the result. These tests run in milliseconds and provide a powerful safety net for your state logic.

## 🧠 **Real-World Case Study: "The Untouchable Checkout Logic"**

- **The System:** A large e-commerce PWA had extremely complex logic in their Redux `checkoutSlice`. It handled shipping costs, taxes, promo codes, gift cards, and more. The reducer function was over 500 lines long.
- **The Fear:** Developers were terrified to touch this file. A small change could have huge financial implications, and it was impossible to manually test every edge case in the browser.
- **The Solution:** A senior engineer dedicated two weeks to writing a comprehensive unit test suite for the `checkoutReducer`. They created tests for every conceivable scenario: applying a percentage discount, applying a fixed-amount discount, calculating tax for different regions, handling an expired promo code, etc. They ended up with over 100 individual test cases.
- **The Result:** The `checkoutSlice` went from being the most feared file in the codebase to the most trusted. The team could now refactor the logic with extreme confidence. If all 100+ tests passed, they _knew_ they hadn't broken anything. The test suite became the official documentation for the business logic.

## 🤔 **Reflective Questions**

1. Why don't we need to test the action creators themselves (like `increment()`)?
2. How would you approach testing an async thunk with `createAsyncThunk`? What would you need to mock?
3. What is "test coverage" and what is a reasonable coverage target for your Redux reducers?

## 📚 **Further Reading**

- **Redux Docs:** [Writing Tests](https://redux.js.org/usage/writing-tests) (The official guide to testing Redux logic).
- **Vitest:** [Official Documentation](https://vitest.dev/) (A modern and fast testing framework for Vite projects).

## 📝 **Mini Task (Production)**

Let's test an async thunk. Assume you have the `fetchTodos` thunk from a previous lesson.

1. Set up a test file for your `todosSlice`.
2. You will need to mock the API call. If you're using `axios`, you can use a library like `vitest-mock-axios` or `jest.mock`.
3. Write a test case for the `fulfilled` state. It should:
    - Dispatch the `fetchTodos` thunk.
    - Mock a successful API response.
    - Assert that the final state has `status: 'succeeded'` and the `items` array contains the data from your mock response.
4. Write a second test case for the `rejected` state.
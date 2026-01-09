# Lesson 4: Unit & Snapshot Testing (Jest)

## Learning Goals

- Configure Jest for React Native.
- Write Unit Tests for functions.
- Use Snapshot Testing.

## 1. Jest Setup

Expo projects come with Jest pre-configured (`jest-expo`).

```bash
npm install -D jest-expo jest
```

## 2. Unit Testing Helper Functions

Pure JavaScript logic.

```javascript
// utils.js
export const isValidEmail = (email) => email.includes("@");

// utils.test.js
import { isValidEmail } from "./utils";

test("validates emails correctly", () => {
  expect(isValidEmail("test@test.com")).toBe(true);
  expect(isValidEmail("broken")).toBe(false);
});
```

## 3. Snapshot Testing

Capture the rendered output of a component structure as a text file. If you accidentally change the UI, the test fails.

```javascript
import React from "react";
import renderer from "react-test-renderer";
import App from "./App";

test("renders correctly", () => {
  const tree = renderer.create(<App />).toJSON();
  expect(tree).toMatchSnapshot();
});
```

## Key Takeaways

- **Unit Tests**: Fast, stable. Test your logic (`utils.js`).
- **Snapshots**: Catches unintended UI changes.
- **Mocking**: You must mock native modules (e.g., `jest.mock('expo-notifications')`) because Jest runs in Node.js, not on a phone.

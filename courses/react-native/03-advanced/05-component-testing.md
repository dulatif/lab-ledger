# Lesson 5: Component Testing (RNTL)

## Learning Goals

- Test Component Interactions (Press, Input).
- Use React Native Testing Library (RNTL).

## 1. Philosophy

Test your component **as a user would**.
Don't test "Is the state `visible` true?".
Test "Can I see the text 'Welcome'?".

```bash
npm install -D @testing-library/react-native
```

## 2. Writing a Test

```jsx
// Button.js
const MyButton = ({ title, onPress }) => (
  <Pressable onPress={onPress}>
    <Text>{title}</Text>
  </Pressable>
);

// Button.test.js
import { render, fireEvent } from "@testing-library/react-native";
import MyButton from "./Button";

test("calls onPress when tapped", () => {
  const mockFn = jest.fn();
  const { getByText } = render(<MyButton title="Click Me" onPress={mockFn} />);

  const element = getByText("Click Me");
  fireEvent.press(element);

  expect(mockFn).toHaveBeenCalled();
});
```

## 3. Querying Elements

- `getByText`: Finds text users see. (Preferred).
- `getByTestId`: Use `testID="my-element"` prop if text is dynamic or hidden. (Fallback).

## Key Takeaways

- **RNTL** runs without an emulator (uses Jest DOM). Fast.
- Focus on **Behavior**, not Implementation details.

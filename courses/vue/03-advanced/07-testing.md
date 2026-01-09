# Lesson 7: Unit Testing

## Learning Goals

- Vitest.
- Vue Test Utils.

## 1. Vitest

A Vite-native unit test runner. Fast.
Compatible with Jest API (`describe`, `it`, `expect`).

## 2. Vue Test Utils

Helper library to mount components in isolation.

```typescript
import { mount } from "@vue/test-utils";
import Counter from "./Counter.vue";
import { expect, test } from "vitest";

test("increments value", async () => {
  const wrapper = mount(Counter);

  // Find button and click
  await wrapper.find("button").trigger("click");

  // Assert text
  expect(wrapper.text()).toContain("Count is: 1");
});
```

## Key Takeaways

- Test **behavior**, not implementation details.
- Don't test framework features (don't test if `v-if` works, test if your logic sets the boolean correctly).

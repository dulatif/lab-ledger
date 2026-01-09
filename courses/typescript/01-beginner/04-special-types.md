# Lesson 4: Special Types

## Learning Goals

- `any`, `unknown`.
- `void`, `never`.
- `null`, `undefined`.

## 1. Top Types

- **any**: Disables type checking. "I know what I'm doing (or I don't care)". **Avoid it**.
- **unknown**: "I don't know what this is yet". You must check the type before using it. **Safer any**.

## 2. Bottom Types

- **never**: Values that never happen (e.g., a function that throws Error or infinite loop).

## 3. Void

- **void**: Returns nothing (for functions).

## Key Takeaways

- Use `unknown` over `any`.
- `any` is viral (it spreads lack of safety to other variables).

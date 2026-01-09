# Lesson 6: E2E Testing (Detox/Maestro)

## Learning Goals

- Understand End-to-End Testing.
- Use Maestro (Newer, easier than Detox).

## 1. What is E2E?

Benchmarks functionality against a **Running App** on a specialized simulator.
It types text, scrolls lists, and taps buttons for real.

- **Pros**: Highest confidence.
- **Cons**: Slow, flaky, requires setup.

## 2. Maestro

A new tool that is much simpler to set up than Detox (which needs complex native modifications). It uses a YAML file to define flows.

**Example `login.yaml`**:

```yaml
appId: com.myapp
---
- launchApp
- tapOn: "Login"
- inputText: "user@example.com"
- tapOn: "Password"
- inputText: "secret"
- tapOn: "Submit"
- assertVisible: "Welcome back!"
```

## 3. Running It

```bash
maestro test login.yaml
```

You will see the emulator mechanically moving and typing.

## Key Takeaways

- Use **Jest/RNTL** for 90% of tests (Fast).
- Use **Maestro/Detox** for "Critical Paths" (Login, Checkout) that must never break.

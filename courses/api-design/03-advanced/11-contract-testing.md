# 11. Contract Testing in Distributed Systems

## 🎯 Learning Goal

Ensuring services speak the same language.

## 🧠 Concept

Unit tests check internal logic. Integration tests check real connections (slow/flaky).
**Contract Tests (Pact)**: Check if Provider matches Consumer expectations.

## 💻 Implementation

1.  Consumer says: "I expect `id` to be a UUID".
2.  Provider build pipeline fails if it changes `id` to `INT`.

## 🧩 Activity / Challenge

1.  Prevents "Whoops, I renamed the field and broke the frontend".

## 🔑 Key Takeaways

- Essential for large microservice teams decoupling deployments.

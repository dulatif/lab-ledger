# 18. Durable Providers 2: i18n

## 🎯 Learning Goal

Use the same Durable Provider concept for Internationalization (i18n).

## 🧠 Concept

Use Case:

- We have a `TranslationService` that loads a huge JSON file (`en.json`, `fr.json`).
- We don't want to load all languages into memory.
- We don't want to check `req.headers['accept-language']` manually in every method.

Solution:

- `TranslationService` is **Durable**.
- Aggregation Key: `fr`, `en`, `es`.
- Result: We have exactly 1 instance of `TranslationService` for 'English users', 1 for 'French users'.

## 💻 Implementation

Same strategy logic, but we extract `Accept-Language` header instead of `X-Tenant-Id`.

```typescript
// extract-language.strategy.ts
const lang = request.headers["accept-language"];
// return subtree based on lang
```

## 🧩 Activity / Challenge

1.  Think about how efficient this is.
2.  Even with 1 million users, if you only support 3 languages, you only have 3 instances of the service!
3.  Much better than Standard Request Scope (1 million instances).

## 🔑 Key Takeaways

- **Context Awareness**: Use Durable Providers whenever you need a service that depends on a _Category_ of request (Tenant, Language, Channel), not the Request itself.

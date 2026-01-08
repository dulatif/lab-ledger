# 3. Code First vs Schema First

## 🎯 Learning Goal

Why choose Schema First?

## 🧠 Concept

**Schema First**:

1.  Design exactly what the Client needs.
2.  Write `schema.graphql`.
3.  Implement resolvers to match.

- _Pros_: Clear contract. No "implementation details" leaking into API.
- _Cons_: You must keep SDL and TS in sync (NestJS helps with this).

## 💻 Implementation

We are doing Schema First in this chapter.

## 🧩 Activity / Challenge

1.  Acknowledge that `Code First` is usually faster for full-stack JS teams.
2.  Acknowledge that `Schema First` is better for strict governance.

## 🔑 Key Takeaways

- Neither is "Better". It depends on your team workflow.

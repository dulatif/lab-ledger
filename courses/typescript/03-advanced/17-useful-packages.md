# 17. Useful Packages in Node.js Ecosystem

## 🎯 Learning Goal

Libraries that leverage TS best.

## 🧠 Concept

Some libraries are "TS First".

## 💻 Recommendations

1.  **Zod / Yup**: Runtime schema validation (infers types from schema).
    ```typescript
    const UserSchema = z.object({ name: z.string() });
    type User = z.infer<typeof UserSchema>;
    ```
2.  **TypeORM / Prisma**: Typed Database access.
3.  **tRPC**: Typed communication between Backend and Frontend.
4.  **ts-pattern**: Better switch statements (Pattern Matching).

## 🧩 Activity / Challenge

1.  Look up `Zod`.
2.  See how it bridges the gap between Runtime (User Input) and Compile Time (Static Types).

## 🔑 Key Takeaways

- The best TS experience comes from using libraries designed for TS.

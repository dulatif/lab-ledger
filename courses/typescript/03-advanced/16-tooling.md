# 16. Tooling (Linting, Formatting, Build Tools)

## 🎯 Learning Goal

Productivity ecosystem.

## 🧠 Concept

TS needs friends.

- **ESLint**: Checks code quality (use `@typescript-eslint`).
- **Prettier**: Formatting.
- **tsc**: The official compiler.
- **esbuild/swc**: Fast compilers (strip types only).

## 💻 Implementation

### tsconfig.json strictness

Enable `"strict": true` (includes `noImplicitAny`, `strictNullChecks`).
Enable `"noUncheckedIndexedAccess"` for extra safety with arrays.

## 🧩 Activity / Challenge

1.  Install `typescript-eslint`.
2.  Configure a rule (e.g., no explicit `any`).
3.  Violate it and see the squiggly line.

## 🔑 Key Takeaways

- Compiler checks types. Linter checks style/bugs. You need both.

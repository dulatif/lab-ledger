# 14. TypeScript Module System and External Modules

## 🎯 Learning Goal

Organize code and consume libraries.

## 🧠 Concept

- **ES Modules**: `import` / `export`. Standard.
- **CommonJS**: `require` / `module.exports`. Legacy/Node.

TS supports both via `module` compiler option.

### Ambient Modules (`.d.ts`)

Types usually live in `@types/package`.
If a library has no types, you might see `Cannot find module 'X'`.

## 💻 Implementation

### Declaring a missing module

Create `types.d.ts`:

```typescript
declare module "my-untyped-lib" {
  export function doSomething(): void;
}
```

## 🧩 Activity / Challenge

1.  Imagine you installed `legacy-js-lib`.
2.  Create a `.d.ts` file to silence the TS error and provide typing for one function.

## 🔑 Key Takeaways

- "Ambient" declaration means "It exists in the environment, trust me".

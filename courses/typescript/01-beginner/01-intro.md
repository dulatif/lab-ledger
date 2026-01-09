# Lesson 1: Introduction & Compilation

## Learning Goals

- What is TypeScript?
- `tsc`.

## 1. What is TypeScript (TS)?

A **Calculated Superset** of JavaScript.

- **Safety**: Statically typed (catches errors at compile time, not runtime).
- **Tooling**: Great Autocomplete and Refactoring.
- **Erasure**: Turns into plain JS when compiled.

## 2. Hello World

1.  Install: `npm install -g typescript`
2.  Write `hello.ts`:
    ```typescript
    const msg: string = "Hello World";
    console.log(msg);
    ```
3.  Compile: `tsc hello.ts` -> Generates `hello.js`.
4.  Run: `node hello.js`.

## Key Takeaways

- Browsers don't run TS. They run the JS that TS produces.

# Lesson 2: Configuration

## Learning Goals

- `tsconfig.json`.
- Strict Mode.

## 1. Init

Run `tsc --init`. Creates `tsconfig.json`.

## 2. Key Options

- `"target": "es2016"`: Which JS version to output? (e.g., arrow functions vs functions).
- `"module": "commonjs"`: Module system (CJS vs ESM).
- `"outDir": "./dist"`: Where to put `.js` files.
- `"strict": true`: Enable all strict type-checking options. **ALWAYS DO THIS**.

## 3. Strict Mode

Includes:

- `noImplicitAny`: You must type arguments.
- `strictNullChecks`: `null` is not in the domain of `string` or `number`.

## Key Takeaways

- Without `strict: true`, TS is just a linter.
- Treat `tsconfig.json` as the project's constitution.

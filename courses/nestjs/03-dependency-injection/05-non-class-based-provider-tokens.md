# 5. Non-class-based Provider Tokens

## 🎯 Learning Goal

Learn best practices for using tokens that aren't classes, avoiding "Magic Strings" by using symbols or `InjectionToken`.

## 🧠 Concept

In the previous lesson, we used a string `'CONNECTION_STRING'` as a token. This works, but it's risky ⚠️.

- **Typos**: If you type `'CONECTION_STRING'` (missing 'n') in one place, your app will crash with a confusing error.
- **Collisions**: If you use a generic name like `'CONFIG'`, a third-party library might also use `'CONFIG'` and overwrite your provider!

To fix this, NestJS allows using **Symbols** or TypeScript interfaces (though interfaces disappear at runtime, so we can't use them directly as tokens).

## 💻 Implementation

### 1. Using Symbols (Better)

Symbols are guaranteed to be unique. Even if you create two symbols with the description "CONFIG", they are different.

```typescript
export const CONNECTION_SYMBOL = Symbol('CONNECTION_SYMBOL');

// Module
providers: [{ provide: CONNECTION_SYMBOL, useValue: '...' }]

// Service
constructor(@Inject(CONNECTION_SYMBOL) db: string) {}
```

### 2. NestJS Style strings (Best for readability)

Often we just export constant strings to avoid typos.

```typescript
// src/constants.ts
export const CATS_REPO_TOKEN = "CATS_REPOSITORY_TOKEN";
```

### 3. Combining Interface + Token

Since Interfaces don't exist at runtime, we can't do `provide: MyInterface`. But we usually want to enforce the shape of what we are injecting.

Pattern:

1.  Define an Interface.
2.  Define a constant _String_ Token.
3.  Inject using the _Token_, but type it as the _Interface_.

```typescript
// interface
export interface ICatsRepository {
  findAll(): string[];
}

// constants.ts
export const CATS_REPO = 'CATS_REPO';

// module.ts
const mockRepo: ICatsRepository = { findAll: () => [] };
providers: [
  { provide: CATS_REPO, useValue: mockRepo }
]

// service.ts
constructor(
  @Inject(CATS_REPO) private readonly catsRepo: ICatsRepository
) {}
```

## 🧩 Activity / Challenge

1.  Refactor your previous `'APP_CONFIG'` example.
2.  Create a file `app.constants.ts`.
3.  Export a constant `export const APP_CONFIG_TOKEN = 'APP_CONFIG_TOKEN'`.
4.  Use this constant in both your Module configuration and your `@Inject()` decorator.
5.  Try changing the string value in the constant file and see that everything still works (because the reference is shared).

## 🔑 Key Takeaways

- Avoid raw "magic strings" in your code.
- Use exported constants or **Symbols** to define tokens.
- This prevents typo-related bugs and naming collisions.
- You can type properties with an Interface while injecting with a String Token.

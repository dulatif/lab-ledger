# 13. Decorators for Metaprogramming

## 🎯 Learning Goal

Annotate and modify classes.

## 🧠 Concept

Decorators are special functions that can hook into Class declarations, methods, properties, or parameters.
_Note: Make sure experimentalDecorators is enabled in tsconfig, or use the new Stage 3 standard (TS 5.0+)._

## 💻 Implementation (Stage 3 Standard)

```typescript
function Log(originalMethod: any, context: ClassMethodDecoratorContext) {
  return function replacementMethod(this: any, ...args: any[]) {
    console.log(`Calling ${String(context.name)}`);
    const result = originalMethod.call(this, ...args);
    return result;
  };
}

class Person {
  @Log
  greet() {
    console.log("Hello!");
  }
}
```

## 🧩 Activity / Challenge

1.  Create a `@Deprecated` decorator that logs a warning when the method is called.

## 🔑 Key Takeaways

- Heavily used in NestJS, Angular, and TypeORM.

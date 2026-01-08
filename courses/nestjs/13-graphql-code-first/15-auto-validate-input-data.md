# 15. Auto-validate Input Data

## 🎯 Learning Goal

Ensure inputs meet requirements.

## 🧠 Concept

Same as REST: use `ValidationPipe` + `class-validator`.

## 💻 Implementation

1.  Global Pipes in `main.ts` or `AppModule`.
    ```typescript
    app.useGlobalPipes(new ValidationPipe());
    ```
2.  Decorate Input:
    ```typescript
    @InputType()
    export class CreateCoffeeInput {
      @MinLength(3)
      @Field()
      name: string;
    }
    ```

## 🧩 Activity / Challenge

1.  Try to create a coffee with a 1-character name.
2.  Playground returns: `"message": ["name must be longer than or equal to 3 characters"]`.

## 🔑 Key Takeaways

- Validation works identically in GraphQL and REST in NestJS.

# 8. Creating Custom Pipes

## 🎯 Learning Goal

Create a custom Pipe to validate input parameters or transform them (e.g., parsing a string into an integer/boolean).

## 🧠 Concept

Pipes have two use cases:

1.  **Transformation**: Input data -> Desired Output (e.g., String "1" -> Number 1).
2.  **Validation**: Evaluate input. Throw error if invalid.

If a Pipe throws an exception, the Controller method is **never executed**.

## 💻 Implementation

### A ParseInt Pipe

Nest has this built-in, but let's build a simple version to understand it.

```typescript
import {
  PipeTransform,
  Injectable,
  ArgumentMetadata,
  BadRequestException,
} from "@nestjs/common";

@Injectable()
export class ParseIntPipe implements PipeTransform {
  transform(value: string, metadata: ArgumentMetadata): number {
    const val = parseInt(value, 10);

    // 🛑 Validation Logic
    if (isNaN(val)) {
      throw new BadRequestException("Validation failed: ID must be a number");
    }

    // ✅ Transformation Logic
    return val;
  }
}
```

### Usage

```typescript
@Get(':id')
findOne(@Param('id', ParseIntPipe) id: number) {
  // 'id' is guaranteed to be a number here
  console.log(typeof id); // 'number'
}
```

## 🧩 Activity / Challenge

1.  Create `FreezePipe` that takes an object body and calls `Object.freeze(value)` on it.
2.  Apply it to a `@Post()` endpoint.
3.  Verify inside the controller that the object is indeed frozen (try to mutate it, strict mode might throw).

## 🔑 Key Takeaways

- Pipes implement `PipeTransform`.
- The `transform()` method returns the value passed to the controller handler.
- Use Pipes to keep validation logic out of your business logic.

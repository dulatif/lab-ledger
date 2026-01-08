# 3. Decorating Model Properties

## 🎯 Learning Goal

Learn how to manually override or enhance the documentation for specific DTO properties using `@ApiProperty`.

## 🧠 Concept

The CLI plugin is great, but sometimes it guesses wrong or misses context.

- "This string is actually an Enum."
- "This generic number is actually an age (min 0)."
- "This field is optional."

We use `@ApiProperty` to provide this metadata explicitly.

## 💻 Implementation

### 1. Basic Usage

```typescript
import { ApiProperty } from "@nestjs/swagger";

export class CreateCatDto {
  @ApiProperty({
    description: "The name of the cat",
    example: "Garfield",
  })
  name: string;

  @ApiProperty({ minimum: 0, default: 1 })
  age: number;
}
```

### 2. Optional Fields

With the CLI plugin, `age?: number` is marked optional automatically.
Without the plugin (or for explicit control):

```typescript
@ApiProperty({ required: false })
breed?: string;
```

### 3. Enums

```typescript
enum Role { User = 'user', Admin = 'admin' }

@ApiProperty({ enum: Role })
role: Role;
```

## 🧩 Activity / Challenge

1.  Take your existing DTO.
2.  Add a description and an example value to one field.
3.  Refresh Swagger UI.
4.  Click "Try it out" and observe that the request body is pre-filled with your example value!

## 🔑 Key Takeaways

- `@ApiProperty` takes an options object.
- Useful for `description`, `example`, `deprecated`, and validation constraints (`minimum`, `maximum`).
- Manual decorators override the CLI plugin's auto-generated metadata.

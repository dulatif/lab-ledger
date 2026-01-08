# 5. Creating a Mongoose Model

## 🎯 Learning Goal

Define the shape of your data.

## 🧠 Concept

In TypeORM, we used `@Entity`.
In Mongoose, we use `@Schema`.

A **Schema** maps to a **Collection**.
A **Model** is the fancy constructor that wraps the schema.

## 💻 Implementation

```typescript
import { Schema, Prop, SchemaFactory } from "@nestjs/mongoose";
import { Document } from "mongoose";

// 1. Define the Class (Typescript Type)
@Schema()
export class Coffee extends Document {
  @Prop()
  name: string;

  @Prop()
  brand: string;

  @Prop([String]) // Array of strings
  flavors: string[];
}

// 2. Generate the Mongoose Schema (The "Magic" part)
export const CoffeeSchema = SchemaFactory.createForClass(Coffee);
```

## 🧩 Activity / Challenge

1.  Create `coffee.schema.ts`.
2.  Note that `extends Document` adds properties like `_id` and `__v` to your typing.

## 🔑 Key Takeaways

- `SchemaFactory.createForClass` reads the metadata and generates the raw Mongoose schema object.

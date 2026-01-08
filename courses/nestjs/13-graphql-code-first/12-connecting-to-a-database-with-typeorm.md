# 12. Connecting to a Database with TypeOrm

## 🎯 Learning Goal

Persist data to Postgres.

## 🧠 Concept

We've done this in Chapter 2.
We need `@nestjs/typeorm` and `pg`.

## 💻 Implementation

1.  `npm install @nestjs/typeorm typeorm pg`.
2.  Configure `TypeOrmModule.forRoot()` in `AppModule`.
3.  **Crucial Step**: Annotate your `Coffee` ObjectType with `@Entity()` too!

```typescript
@ObjectType() // GraphQL
@Entity() // TypeORM
export class Coffee {
  @Field((type) => Int) // GraphQL
  @PrimaryGeneratedColumn() // TypeORM
  id: number;
}
```

## 🧩 Activity / Challenge

1.  Start a Postgres container (`docker run...`).
2.  Configure the connection.
3.  Dual-annotate the Coffee class.

## 🔑 Key Takeaways

- It is common to mix `@ObjectType` and `@Entity` on the same class to avoid duplication (Code First approach).
- However, sometimes you want separate classes if the DB schema differs greatly from the API schema.

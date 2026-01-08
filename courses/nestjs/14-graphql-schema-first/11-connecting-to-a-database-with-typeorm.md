# 11. Connecting to a Database with TypeOrm

## 🎯 Learning Goal

Setup Postgres.

## 🧠 Concept

We need a `Coffee` Entity.
**Crucial**: In Code First, one class served as both ObjectType and Entity.
In Schema First, we usually have:

1.  `coffee.graphql` (Schema).
2.  `coffee.entity.ts` (DB Entity).
3.  `coffee.interface.ts` (Generated from GQL).

## 💻 Implementation

```typescript
@Entity()
export class Coffee {
  @PrimaryGeneratedColumn()
  id: number;
}
```

## 🧩 Activity / Challenge

1.  Setup `TypeOrmModule`.
2.  Create the Entity.
3.  Note that the Entity has `number` ID, but GQL expects `ID` (string). NestJS usually handles `number` -> `ID` serialization fine, but be aware.

## 🔑 Key Takeaways

- Schema First often leads to "Two classes for the same thing" (Entity vs GQL Type).

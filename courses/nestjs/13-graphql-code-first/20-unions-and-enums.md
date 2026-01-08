# 20. Unions and Enums

## 🎯 Learning Goal

More Polymorphism and Constants.

## 🧠 Concept

**Union**: "Result is either A or B" (No shared fields required).
**Enum**: Restricted set of strings.

## 💻 Implementation

### Enum

```typescript
export enum CoffeeType {
  ARABICA = "arabica",
  ROBUSTA = "robusta",
}
registerEnumType(CoffeeType, { name: "CoffeeType" });
```

### Union

```typescript
export const ResultUnion = createUnionType({
  name: "ResultUnion",
  types: () => [Coffee, Tea],
});
```

## 🧩 Activity / Challenge

1.  Add a `type` field to Coffee using the Enum.
2.  Create a search query returning a Union.

## 🔑 Key Takeaways

- Enums must be registered with `registerEnumType` to generate schema definitions.

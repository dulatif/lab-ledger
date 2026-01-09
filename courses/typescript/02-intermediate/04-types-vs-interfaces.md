# 4. Deep Dive: Types vs Interfaces

## 🎯 Learning Goal

Settle the debate: Which one to use?

## 🧠 Concept

Functionally, they are 95% similar.

- **Interfaces**: Can participate in Declaration Merging. Better error messages in some IDEs.
- **Types**: Can represent Primitives, Unions, Tuples. Cannot be merged.

## 💻 Implementation

### When to use Interface

1.  Defining Object shapes (Models, DTOs).
2.  Authoring libraries (allows users to extend definitions).

### When to use Type

1.  Unions (`type ID = string | number`).
2.  Tuples (`type Row = [number, string]`).
3.  Complex transformations (`Mapped Types`).

## 🧩 Activity / Challenge

1.  Refactor a previous code snippet from Interface to Type Alias.
2.  Try to merge two Types (It fails).

## 🔑 Key Takeaways

- **Rule of Thumb**: Use `interface` for Objects/API responses. Use `type` for everything else (Unions, Primitives, Functions).

Next: [[05-union-types-and-aliases]]
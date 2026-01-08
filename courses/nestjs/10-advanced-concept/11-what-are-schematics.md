# 11. What are Schematics?

## 🎯 Learning Goal

Understand the technology behind the Nest CLI (`nest g controller`).

## 🧠 Concept

**Schematics** is a library (from Angular team) for code generation and modification.
It treats your file system as a Tree.

1.  Read Tree.
2.  Apply Transformations (Add file, modify JSON).
3.  Commit changes to disk.

It is strictly **functional** and **safe**. It runs in a virtual file system (dry-run) before touching real files.

## 💻 Implementation

You don't "implement" schematics in your main app. You create a separate **CLI Project**.

## 🧩 Activity / Challenge

1.  Run `nest g controller users --dry-run`.
2.  This executes a schematic called `controller`.
3.  It previews the changes without writing them. This is the power of Schematics Abstract Syntax Tree (AST) manipulation.

## 🔑 Key Takeaways

- **Standardization**: Enforces coding standards across teams.
- **Automation**: Removes repetitive boilerplate setup.

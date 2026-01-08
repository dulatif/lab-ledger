# 12. Adding Custom Schematics

## 🎯 Learning Goal

Create your own `nest g` command.

## 🧠 Concept

Imagine your company has a strict rule: "Every Controller must have a specific Header Decorator".
You can write a schematic `nest g my-company-controller` that generates it correctly every time.

## 💻 Implementation

### 1. Install CLI tools

```bash
npm install -g @angular-devkit/schematics-cli
```

### 2. Create Collection

```bash
schematics blank my-schematics
```

### 3. Define Rule

```typescript
// factory.ts
export function myController(options: any): Rule {
  return (tree: Tree, _context: SchematicContext) => {
    // Logic to create files from templates...
    return tree;
  };
}
```

## 🧩 Activity / Challenge

1.  This is a deep dive topic.
2.  The challenge is to browse the [NestJS CLI source code](https://github.com/nestjs/nest-cli) to see how they implemented `controller` generation.

## 🔑 Key Takeaways

- **DX (Developer Experience)**: Investing in Schematics improves the life of everyone on your team.

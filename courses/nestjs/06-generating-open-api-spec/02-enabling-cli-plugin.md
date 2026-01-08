# 2. Enabling CLI Plugin

## 🎯 Learning Goal

Automate the documentation process so you don't have to manually decorate every single DTO property.

## 🧠 Concept

By default, NestJS Swagger requires you to add `@ApiProperty()` to every field in your DTOs so it knows their type.
This is **tedious** and breaks the DRY (Don't Repeat Yourself) principle. 😫

**Good News**: The NestJS CLI Plugin can automatically inspect your TypeScript types and add these decorators for you during the build process!

## 💻 Implementation

### 1. Update nest-cli.json

Open `nest-cli.json` in your project root. Add `compilerOptions`.

```json
{
  "$schema": "https://json.schemastore.org/nest-cli",
  "collection": "@nestjs/schematics",
  "sourceRoot": "src",
  "compilerOptions": {
    "deleteOutDir": true,
    "plugins": ["@nestjs/swagger"] // 👈 Add this line!
  }
}
```

### 2. Restart Build

You must stop and restart your `npm run start:dev` for the CLI settings to take effect.

### 3. Verification

Previously, a DTO like this would be invisible in Swagger:

```typescript
class CreateCatDto {
  name: string;
  age: number;
}
```

With the plugin, Swagger now automatically detects `name` (string) and `age` (number)!

## 🧩 Activity / Challenge

1.  Check your `nest-cli.json`.
2.  Enable the plugin.
3.  Create a `CreateCatDto` WITHOUT any `@ApiProperty` decorators.
4.  Use it in a controller `@Post(@Body() dto: CreateCatDto)`.
5.  Check Swagger UI. You should see the schema fully populated.

## 🔑 Key Takeaways

- The **CLI Plugin** introspects TypeScript files.
- It automatically adds `@ApiProperty` for DTO properties.
- It automatically detects response types for Controller methods.

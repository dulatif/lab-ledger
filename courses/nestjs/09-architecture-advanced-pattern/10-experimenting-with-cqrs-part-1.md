# 10. Experimenting with CQRS. Part 1

## 🎯 Learning Goal

Implement the **Write Side** using `@nestjs/cqrs`. We will create a Command to register a user.

## 🧠 Concept

Instead of `UserService.register(email, password)`, we define a class `RegisterUserCommand`.
This command is a data packet describing user's intent.

## 💻 Implementation

### 1. Define Command

```typescript
// src/user/commands/register-user.command.ts
export class RegisterUserCommand {
  constructor(public readonly email: string, public readonly pass: string) {}
}
```

### 2. Define Handler

This replaces the "Service Method".

```typescript
// src/user/commands/handlers/register-user.handler.ts
@CommandHandler(RegisterUserCommand)
export class RegisterUserHandler
  implements ICommandHandler<RegisterUserCommand>
{
  constructor(private repo: UserRepository) {}

  async execute(command: RegisterUserCommand) {
    const { email, pass } = command;
    // Business Logic here...
    console.log(`Creating user ${email}`);
  }
}
```

### 3. Controller

```typescript
@Post()
async register(@Body() dto: CreateUserDto) {
  return this.commandBus.execute(
    new RegisterUserCommand(dto.email, dto.password)
  );
}
```

## 🧩 Activity / Challenge

1.  Install `@nestjs/cqrs`.
2.  Create the Command and Handler.
3.  Register the Handler in `providers`.
4.  Inject `CommandBus` in Controller and trigger it.

## 🔑 Key Takeaways

- **Decoupling**: The controller doesn't know _who_ handles the command or _how_.
- **Single Responsibility**: Each Handler does exactly one thing.

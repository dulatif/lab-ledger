# 4. Creating our first GraphQL Application

## 🎯 Learning Goal

Boot the GraphQL server.

## 🧠 Concept

Configure `GraphQLModule` in `AppModule`.
We must specify where to output the auto-generated schema.

## 💻 Implementation

```typescript
import { ApolloDriver, ApolloDriverConfig } from "@nestjs/apollo";

@Module({
  imports: [
    GraphQLModule.forRoot<ApolloDriverConfig>({
      driver: ApolloDriver,
      autoSchemaFile: join(process.cwd(), "src/schema.gql"),
    }),
  ],
})
export class AppModule {}
```

## 🧩 Activity / Challenge

1.  Run the app.
2.  It might **FAIL**.
3.  Why? "Query root type must be provided."
4.  GraphQL _requires_ at least one Query. We haven't defined any.

## 🔑 Key Takeaways

- `autoSchemaFile: true` generates in memory. Providing a path writes it to disk (useful for sharing with frontend).

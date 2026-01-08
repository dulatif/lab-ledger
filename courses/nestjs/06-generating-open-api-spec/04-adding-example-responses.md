# 4. Adding Example Responses

## 🎯 Learning Goal

Document the possible HTTP responses (200, 403, 404) so API consumers know what to expect.

## 🧠 Concept

By default, Swagger assumes a 200 OK.
But your API might return `404 Not Found` or `400 Bad Request`.
Documenting these error states is critical for a good developer experience (DX).

## 💻 Implementation

### 1. Valid Success Response

Use `@ApiResponse` or specific shortcuts like `@ApiOkResponse`, `@ApiCreatedResponse`.

```typescript
@Post()
@ApiCreatedResponse({
  description: 'The record has been successfully created.',
  type: Cat
})
create(@Body() createCatDto: CreateCatDto) {}
```

### 2. Error Responses

```typescript
@Get(':id')
@ApiOkResponse({ description: 'Found the cat', type: Cat })
@ApiNotFoundResponse({ description: 'Cat not found' })
@ApiForbiddenResponse({ description: 'Forbidden.' })
findOne(@Param('id') id: string) {}
```

## 🧩 Activity / Challenge

1.  Go to a `findOne` endpoint.
2.  Add `@ApiNotFoundResponse({ description: 'Entity does not exist' })`.
3.  Check Swagger UI. You will see a new entry under "Responses" for code 404.

## 🔑 Key Takeaways

- `@ApiResponse({ status: 200 })`: Genetic decorator.
- `@ApiOkResponse`, `@ApiBadRequestResponse`, etc.: Specific semantic decorators.
- Always specify the `type` if returning a DTO/Entity so Swagger generates the schema model.

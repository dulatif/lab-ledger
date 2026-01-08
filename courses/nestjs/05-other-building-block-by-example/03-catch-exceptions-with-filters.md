# 3. Catch Exceptions with Filters

## 🎯 Learning Goal

Learn how to create a custom Exception Filter to catch errors and standardize the error response format across your API.

## 🧠 Concept

By default, NestJS sends a JSON response when an error occurs:

```json
{
  "statusCode": 500,
  "message": "Internal server error"
}
```

But maybe you want a text timestamp? Or a custom error code field? Or you want to log the error to a file?
**Exception Filters** allow you to take control of the Response object when an exception is thrown.

## 💻 Implementation

### 1. Create the Filter

Implement `ExceptionFilter` and decorate with `@Catch(HttpException)`.

```typescript
// src/common/filters/http-exception.filter.ts
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
  HttpException,
} from "@nestjs/common";
import { Request, Response } from "express";

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();
    const status = exception.getStatus();

    // 🕸️ Custom JSON Response
    response.status(status).json({
      statusCode: status,
      timestamp: new Date().toISOString(),
      path: request.url,
      message: exception.message,
    });
  }
}
```

### 2. Bind the Filter

Let's apply it globally in `main.ts`.

```typescript
app.useGlobalFilters(new HttpExceptionFilter());
```

## 🧩 Activity / Challenge

1.  Create the `HttpExceptionFilter` above.
2.  Throw a `new NotFoundException('Oops')` in one of your controllers.
3.  Check the response in Postman/Browser. Does it have the `timestamp` and `path`?
4.  If yes, you successfully intercepted the error!

## 🔑 Key Takeaways

- **@Catch()**: Tells NestJS which exceptions this filter handles. Catching `HttpException` catches standard API errors.
- **ArgumentsHost**: A utility to get the Request and Response objects (abstracts away Http/Ws/Rpc contexts).
- Filters are the **last** line of defense before the response leaves the server.

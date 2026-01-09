# Lesson 2: HTTP Client

## Learning Goals

- Setup `HttpClient`.
- GET and POST data.

## 1. Setup

In a standalone app, you must provide the HTTP client in `app.config.ts`.

```typescript
import { provideHttpClient } from "@angular/common/http";

export const appConfig: ApplicationConfig = {
  providers: [provideHttpClient()],
};
```

## 2. Making Requests

Requests return **Observables**. You must `.subscribe()` to them to trigger the request.

```typescript
@Injectable({ providedIn: "root" })
export class DataService {
  private http = inject(HttpClient);

  getPosts() {
    return this.http.get<Post[]>("https://api.example.com/posts");
  }

  createPost(post: Post) {
    return this.http.post("https://api.example.com/posts", post);
  }
}
```

## 3. Consuming in Component

```typescript
export class BlogComponent implements OnInit {
  posts: Post[] = [];
  dataService = inject(DataService);

  ngOnInit() {
    this.dataService.getPosts().subscribe({
      next: (data) => (this.posts = data),
      error: (err) => console.error(err),
    });
  }
}
```

## Key Takeaways

- **Http** methods return Observables (Cold). Nothing happens until you subscribe.
- Always type your responses (`get<Type>`).

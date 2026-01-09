# Angular Intermediate Course Syllabus

## Module 4: Dependency Injection & Services

**Goal**: Manage business logic and data.

1.  **Dependency Injection (DI)**
    - Concepts: Singleton Pattern, Providers, `providedIn: 'root'`.
    - Goal: Share logic across components.
2.  **HTTP Client**
    - Concepts: `HttpClientModule` (or `provideHttpClient`), Observables basics.
    - Goal: Fetch data from a REST API.
3.  **Interceptors**
    - Concepts: Modifying requests globally (e.g., Auth Tokens).
    - Goal: Attach an API key to every request.

## Module 5: Routing & Forms

**Goal**: Build complex multi-page applications.

4.  **Routing Basics**
    - Concepts: `RouterModule`, `<router-outlet>`, `routerLink`.
    - Goal: Navigate between pages.
5.  **Template-Driven Forms**
    - Concepts: `FormsModule`, `[(ngModel)]` (Two-way binding).
    - Goal: Build a simple login form.
6.  **Reactive Forms**
    - Concepts: `ReactiveFormsModule`, `FormControl`, `FormGroup`, Validators.
    - Goal: Build a complex registration form with validation.

## Module 6: RxJS & Reactivity

**Goal**: Master asynchronous data streams.

7.  **RxJS Basics**
    - Concepts: Observables vs Promises, Subscription management.
    - Goal: Understand the "Stream" mental model.
8.  **Operators**
    - Concepts: `map`, `filter`, `tap`, `switchMap`.
    - Goal: Transform data streams elegantly.
9.  **Async Pipe**
    - Concepts: `| async` in templates.
    - Goal: Subscribe to data without manual cleanup.

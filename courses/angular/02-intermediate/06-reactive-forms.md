# Lesson 6: Reactive Forms

## Learning Goals

- Use `ReactiveFormsModule`.
- Build complex forms with `FormGroup` and `FormControl`.

## 1. Setup

Scalable forms where logic is defined in TypeScript.
Requires `ReactiveFormsModule`.

## 2. Defining the Form

```typescript
form = new FormGroup({
  email: new FormControl("", [Validators.required, Validators.email]),
  password: new FormControl("", [Validators.required, Validators.minLength(8)]),
  address: new FormGroup({
    street: new FormControl(""),
    city: new FormControl(""),
  }),
});
```

## 3. Binding in HTML

```html
<form [formGroup]="form" (ngSubmit)="submit()">
  <input formControlName="email" type="email" />

  <div formGroupName="address">
    <input formControlName="street" />
  </div>

  <button type="submit" [disabled]="form.invalid">Submit</button>
</form>
```

## 4. Typed Forms

In Angular 14+, forms are strictly typed.
`form.value.email` is known to be `string | null`.

## Key Takeaways

- **Reactive Forms** give you direct access to the form object in TS.
- Easier to unit test.
- Support dynamic changes (adding/removing fields).

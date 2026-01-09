# Lesson 5: Template-Driven Forms

## Learning Goals

- Use `FormsModule`.
- Implement `[(ngModel)]`.

## 1. Setup

Simple forms where logic is mostly in HTML.
Requires importing `FormsModule`.

## 2. The Model

```typescript
user = {
  name: '',
  favoriteColor: 'Blue'
};

submit() {
  console.log(this.user);
}
```

## 3. The Template

```html
<form #myForm="ngForm" (ngSubmit)="submit()">
  <input
    name="userName"
    [(ngModel)]="user.name"
    required
    minlength="3"
    #nameInp="ngModel"
  />

  <div *ngIf="nameInp.invalid && nameInp.touched">Name is required.</div>

  <button [disabled]="myForm.invalid">Save</button>
</form>
```

## Key Takeaways

- **[(ngModel)]**: Syncs the input value with the class property instantly.
- **References (#)**: allow you to check the validity state (`valid`, `pristine`, `touched`) of the form or control.
- Good for simple login/signup forms.

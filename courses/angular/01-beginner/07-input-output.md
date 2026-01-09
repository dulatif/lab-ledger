# Lesson 7: Input & Output

## Learning Goals

- Parent to Child Communication (`@Input`).
- Child to Parent Communication (`@Output`).

## 1. Passing Data Down (@Input)

The Child declares it expects data.

```typescript
// user-card.component.ts
export class UserCardComponent {
  @Input() name: string = ""; // Legacy
  // OR New Signal Input (Angular 17.1+)
  // name = input<string>('');
}
```

**Parent HTML**:

```html
<app-user-card [name]="'Alice'"></app-user-card>
```

## 2. Emitting Events Up (@Output)

The Child notifies the Parent of an action.

```typescript
// user-card.component.ts
export class UserCardComponent {
  @Output() delete = new EventEmitter<void>();

  onDeleteClick() {
    this.delete.emit();
  }
}
```

**Parent HTML**:

```html
<app-user-card (delete)="removeUser()" name="Alice"> </app-user-card>
```

## Key Takeaways

- Data flows **Down** (Input).
- Events flow **Up** (Output).
- Use TypeScript types for your Inputs to prevent errors.

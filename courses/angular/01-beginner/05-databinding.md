# Lesson 5: Data Binding

## Learning Goals

- One-Way Binding (Source -> View).
- Event Binding (View -> Source).
- Property Binding.

## 1. Interpolation {{ }}

Embed text values into HTML.

```html
<h1>Welcome, {{ username }}!</h1>
<p>Sum: {{ 1 + 1 }}</p>
```

## 2. Property Binding [ ]

Bind values to HTML properties (attributes).
Think: "I want to control this property with a variable."

```html
<img [src]="userAvatarUrl" [alt]="username" />
<button [disabled]="isSubmitting">Submit</button>
```

## 3. Event Binding ( )

Listen to user events.

```html
<button (click)="onSave()">Save</button> <input (input)="onTyping($event)" />
```

## 4. Two-Way Binding [( )] (Forms)

Used mostly in Template-Driven Forms.
"Banana in a Box": `[()]` - Listens to events AND updates properties.

```html
<!-- Requires FormsModule import -->
<input [(ngModel)]="username" />
```

## Key Takeaways

- **{{ }}**: Read text.
- **[ ]**: Write to generic property.
- **( )**: Listen to event.

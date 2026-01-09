# Lesson 12: Forms & v-model

## Learning Goals

- Two-way data binding.
- Modifiers (`.lazy`, `.number`).

## 1. Syntax

`v-model` syncs the input's value with a variable.

```html
<input v-model="text" />
<p>{{ text }}</p>
```

It works on `<input>`, `<textarea>`, `<select>`.

## 2. Behind the Scenes

It actually expands to:

```html
<input :value="text" @input="text = $event.target.value" />
```

## 3. Modifiers

- `.lazy`: Sync after "change" event instead of "input" (on blur).
  ```html
  <input v-model.lazy="msg" />
  ```
- `.number`: Automatically typecast to number.
  ```html
  <input v-model.number="age" />
  ```
- `.trim`: Trim whitespace.

## 4. Checkboxes & Selects

Vue handles types intelligently.

```html
<!-- Multiple Checkbox -->
<input type="checkbox" value="Jack" v-model="checkedNames" />
<input type="checkbox" value="John" v-model="checkedNames" />
<!-- checkedNames is ['Jack', 'John'] -->
```

## Key Takeaways

- `v-model` is the standard for form inputs.
- It simplifies state management for forms significantly.

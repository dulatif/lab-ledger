# Lesson 11: Event Handling

## Learning Goals

- Listening to events (`v-on` / `@`).
- Event Modifiers.

## 1. Listening

```html
<button @click="count++">Add 1</button>

<button @click="greet">Greet</button>
```

```typescript
function greet(event) {
  alert("Hello " + event.target.tagName);
}
```

## 2. Passing Arguments

```html
<button @click="say('hello', $event)">Say Hello</button>
```

`$event` is the special variable for the native DOM event.

## 3. Event Modifiers

Common patterns (like `e.preventDefault()`) are built-in.

- `.stop`: `event.stopPropagation()`
- `.prevent`: `event.preventDefault()`
- `.self`: only trigger if `event.target` is self
- `.once`: trigger only once

```html
<!-- Submit form without reloading page -->
<form @submit.prevent="onSubmit"></form>
```

## Key Modifiers

- `@keyup.enter="submit"`
- `@keydown.page-down="onPageDown"`

## Key Takeaways

- Use modifiers to keep your JS logic clean of DOM event details.

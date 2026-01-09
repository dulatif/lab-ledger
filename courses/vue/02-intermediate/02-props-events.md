# Lesson 2: Props & Events

## Learning Goals

- Passing data down (`defineProps`).
- Emitting data up (`defineEmits`).

## 1. Props

Read-only data from parent.

```typescript
// Child.vue
<script setup>
// Macro (no import needed)
const props = defineProps({
  title: String,
  likes: { type: Number, default: 0 }
});

console.log(props.title);
</script>

<template>
  <h1>{{ title }}</h1>
</template>
```

## 2. Emits

Sending messages to parent.

```typescript
// Child.vue
<script setup>
const emit = defineEmits(['delete', 'update']);

function onDelete() {
  emit('delete', 123); // payload
}
</script>
```

**Parent**:

```html
<Child @delete="handleDelete" />
```

## 3. v-model Components

You can implement `v-model` on custom components.
It translates to prop `modelValue` and event `update:modelValue`.

```typescript
// MyInput.vue
defineProps(["modelValue"]);
defineEmits(["update:modelValue"]);
```

```html
<input
  :value="modelValue"
  @input="$emit('update:modelValue', $event.target.value)"
/>
```

## Key Takeaways

- **One-Way Data Flow**: Props down, Events up.
- **Macros**: `defineProps` and `defineEmits` are compiler macros, available only inside `<script setup>`.

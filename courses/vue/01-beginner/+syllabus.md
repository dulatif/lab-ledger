# Vue.js Beginner Course Syllabus

## Module 1: Setup & Essentials

**Goal**: Initialize a project and understand the basic structure.

1.  **Bootstrapping a Project**
    - Concepts: `npm create vue@latest` (Vue CLI replacement), Vite.
    - Goal: Create "Hello World".
2.  **Single File Components (SFC)**
    - Concepts: The `.vue` file format (`<script>`, `<template>`, `<style>`).
    - Goal: Understand the anatomy of a Vue component.
3.  **App Config & Global Properties**
    - Concepts: `createApp`, `app.mount`, Global Error Handling.
    - Goal: Configure the root application instance.

## Module 2: Template Syntax & Directives

**Goal**: Master the View layer.

4.  **Interpolation & Text**
    - Concepts: `{{ }}`, `v-text`, `v-html`.
    - Goal: Display dynamic data.
5.  **Attribute Binding**
    - Concepts: `v-bind` (or simply `:colon`).
    - Goal: Bind dynamic IDs, classes, and styles.
6.  **Static & Optimization**
    - Concepts: `v-once`, `v-pre`.
    - Goal: Perform basic render optimizations.

## Module 3: Reactivity & Logic

**Goal**: Handle Logic and State.

7.  **Reactivity Fundamentals**
    - Concepts: `ref` vs `reactive` (Composition API focus).
    - Goal: Manage local component state.
8.  **Conditional Rendering**
    - Concepts: `v-if` vs `v-show`.
    - Goal: Show/Hide elements based on state.
9.  **List Rendering**
    - Concepts: `v-for`, `:key` importance.
    - Goal: Render arrays of data.
10. **Computed Properties**
    - Concepts: `computed()`.
    - Goal: Derive state efficiently (Caching).

## Module 4: Interaction

**Goal**: Handle User Input.

11. **Event Handling**
    - Concepts: `v-on` (or `@`), Event Modifiers (`.stop`, `.prevent`).
    - Goal: Respond to clicks and keypresses.
12. **Form Input Bindings**
    - Concepts: `v-model` basics.
    - Goal: Create a controlled input.

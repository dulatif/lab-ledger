# Lesson 4: Interpolation & Text

## Learning Goals

- Mustache Syntax `{{ }}`.
- Raw HTML logic.

## 1. Text Interpolation

The most basic form of data binding.

```html
<span>Message: {{ msg }}</span>
```

If `msg` changes in the script, this updates automatically.

## 2. Using JavaScript Expressions

You can use **expressions** (values), not statements.

```html
<!-- Valid -->
{{ number + 1 }} {{ ok ? 'YES' : 'NO' }} {{ message.split('').reverse().join('')
}}

<!-- Invalid -->
{{ var a = 1 }}
<!-- Statement -->
{{ if (ok) { return message } }}
<!-- Flow control -->
```

## 3. Raw HTML (v-html)

By default, `{{ }}` interprets data as plain text (safe).
To render HTML:

```html
<span v-html="rawHtml"></span>
```

**Security Warning**: Only use on trusted content. Never use on user input (XSS risk).

## Key Takeaways

- `{{ }}` is for text.
- JavaScript expressions work inside `{{ }}`.

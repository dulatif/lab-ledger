# Lesson 8: Pipes

## Learning Goals

- Transform data in the template.
- Use Built-in pipes.

## 1. What is a Pipe?

A pipe takes data as input and transforms it to a desired output for display.
Syntax: `value | pipeName`

## 2. Common Built-in Pipes

- `DatePipe`: Formats dates.
  ```html
  <p>Joined: {{ user.joinDate | date:'shortDate' }}</p>
  <!-- Output: 1/15/24 -->
  ```
- `UpperCasePipe` / `LowerCasePipe`:
  ```html
  <h1>{{ title | uppercase }}</h1>
  ```
- `CurrencyPipe`:
  ```html
  <p>Price: {{ price | currency:'USD' }}</p>
  <!-- Output: $19.99 -->
  ```
- `JsonPipe`: Debugging tool. Dumps the object as a string.
  ```html
  <pre>{{ user | json }}</pre>
  ```

## 3. Chaining Pipes

You can apply multiple pipes.

```html
{{ date | date:'fullDate' | uppercase }}
```

## Key Takeaways

- Pipes only change the **Display** aspect of the data. They do not modify the actual property in your component class.
- You must import the specific pipes (e.g., `DatePipe`) in the `imports: []` array of your standalone component.

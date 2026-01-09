# Lesson 1: Introduction to Angular

## Learning Goals

- What is Angular?
- Angular vs React.
- The "Platform" concept.

## 1. What is Angular?

Angular is a **Platform** and framework for building single-page client applications using HTML and TypeScript.
It is written by Google.
Unlike React (a library), Angular is a "batteries-included" framework. It includes:

- A Router.
- An HTTP Client.
- Forms management.
- Internationalization (i18n).
- Testing utilities.

## 2. Key Architecture Concepts

- **Modules (NgModules)**: Containers for a block of code (though Modern Angular uses "Standalone Components" to make this optional).
- **Components**: The fundamental building blocks of the UI.
- **Services**: Containers for logic (data fetching, state) shared across components via **Dependency Injection**.

## 3. TypeScript

Angular is built entirely on TypeScript. You cannot write "standard Angular" in plain JS.

- **Classes**: Every component/service is a class.
- **Decorators**: `@Component`, `@Injectable` describe what the class does.

## Key Takeaways

- Angular is opinionated: There is one "Angular Way" to do things.
- It is robust and suited for Enterprise applications.

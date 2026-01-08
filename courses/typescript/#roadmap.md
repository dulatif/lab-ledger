Based on the `roadmap.sh` file you provided, here is a comprehensive syllabus structured by proficiency level. This curriculum is designed to move from foundational syntax to advanced type system manipulation.

### **Level 1: Beginner (Foundations & Setup)**

**Goal:** Understand the compilation process, configure the environment, and master static typing basics.

**Module 1: Ecosystem & Configuration**

- **Introduction:** Overview of TypeScript and its relationship with JavaScript.

- **Comparisons:** Understanding TypeScript vs. JavaScript and Interoperability.

- **Environment Setup:** Installation and Configuration.

- **The Compiler:** Detailed study of `tsconfig.json` and Compiler Options.

- **Execution:** Running TypeScript using `tsc` , `ts-node` , and the TS Playground.

**Module 2: The Type System (Primitives)**

- **Primitive Types:** working with `boolean` , `number` , and `string`.

- **Empty Types:** Understanding `void` , `null` , and `undefined`.

- **Top & Bottom Types:** Usage of `unknown` , `any` , and `never`.

- **Collections:** Defining `Array` types and fixed-length `Tuple` types.

- **Enumerations:** Working with `Enum`.

**Module 3: Functions & Objects Basics**

- **Object Typing:** Defining structure with Object Types.

- **Functions:** Typing Functions , managing Parameters , and Return Types.

- **Inference:** Understanding how TypeScript automatically assigns types via Type Inference.

---

### **Level 2: Intermediate (Architecture & Logic)**

**Goal:** Build robust applications using interfaces, classes, and control flow analysis.

**Module 4: Interfaces & Type Composition**

- **Interfaces:** Defining TypeScript Interfaces , Interface Declarations , and Hybrid Types.

- **Inheritance:** Extending Interfaces vs. Intersection Types.

- **Comparison:** A deep dive into Types vs. Interfaces.

- **Combining Types:** Using Union Types and Type Aliases.

- **Keys:** Using the `keyof` Operator to extract object keys.

**Module 5: Control Flow & Narrowing**

- **Type Guards:** Implementing Type Guards / Narrowing to handle dynamic data.

- **Runtime Checks:** Using `instanceof` and `typeof`.

- **Logic Checks:** Equality and Truthiness narrowing.

- **Custom Guards:** Creating user-defined Type Predicates.

- **Assertions:** Type Assertions using `as [type]` , `as const` , and Non-null Assertion.

**Module 6: Object-Oriented TypeScript**

- **Classes:** Defining Classes and Constructor Params.

- **Encapsulation:** Managing visibility with Access Modifiers and `readonly`.

- **Abstraction:** Abstract Classes and Method Overriding.

- **Polymorphism:** Inheritance vs. Polymorphism.

- **Overloading:** Function Overloading and Constructor Overloading.

---

### **Level 3: Advanced (Metaprogramming & Tooling)**

**Goal:** Master utility types, generics, and the module system for scalable library development.

**Module 7: Generics**

- **Core Concepts:** Introduction to Generics and Generic Types.

- **Safety:** Implementing Generic Constraints to limit accepted types.

- **Modern Features:** Using the `satisfies` keyword for validation without widening.

**Module 8: Utility Types**

- **Transformation:** `Partial` , `Pick` , `Omit` , and `Record`.

- **Set Operations:** `Exclude` and `Extract`.

- **Function Helpers:** `Parameters` , `ReturnType` , and `InstanceType`.

- **Async:** Handling promises with `Awaited`.

- **Nullability:** Using `NonNullable`.

**Module 9: Advanced Type Manipulation**

- **Logic in Types:** Conditional Types and Type Compatibility rules.

- **Mapping:** Creating new types from old ones with Mapped Types.

- **Literals:** Literal Types and Template Literal Types.

- **Recursion:** Defining Recursive Types.

- **Decorators:** Implementing Decorators for metaprogramming.

**Module 10: Modules & Ecosystem**

- **Module System:** TypeScript Modules , External Modules , and Ambient Modules.

- **Namespaces:** Usage , Namespace Augmentation , and Global Augmentation.

- **Tooling:** Formatting , Linting , and Build Tools.

- **Packages:** Utilizing Useful Packages within the Node.js ecosystem.

**Would you like me to draft a specific lesson plan or a quiz for the "Beginner" level modules to get you started?**

# 💡 AR01.4: Studi Kasus Hierarki Class

**Outline:**

- **The Smell (SEEI):** A rigid inheritance hierarchy that suffers from a "combinatorial explosion" of subclasses.
- **The Refactor (PPP):** A step-by-step deconstruction of the hierarchy, replacing it with a flexible compositional model.
- **Your Refactoring Mission (Production):** Time to perform the full refactoring on your own.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Combinatorial Feature Explosion**
This happens when new, independent features are added to a hierarchy using only subclassing. You're forced to create a new class for every possible combination of features. For example, `PdfReport` and `CsvReport` become `EncryptedPdfReport`, `EncryptedCsvReport`, `CompressedEncryptedPdfReport`, etc. The number of classes explodes.

**The Negative Impact**

- **Unmaintainable Codebase:** The massive number of classes becomes impossible to manage.
- **Code Duplication:** Encryption logic gets repeated across multiple leaf-node classes.
- **Extreme Rigidity:** Adding one new format (e.g., `XML`) requires creating a whole new set of feature-combination subclasses.

**Example of the Smell**
A `Report` hierarchy where format and encryption are handled by subclassing, leading to redundant and complex inheritance chains.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Decompose and Compose**
Stop thinking about what a report _is_ (Inheritance), and start thinking about what it _has_ (Composition).

1. **Identify Orthogonal Features:** Format vs. Encryption.
2. **Extract Strategy Interfaces:** Create `IReportFormatter` and `IEncryptionStrategy`.
3. **Create Concrete Strategies:** Implement `PdfFormatter`, `CsvFormatter`, `AesEncryptor`, and a `NoOpEncryptor`.
4. **Compose the Context:** Update the `Report` class to accept these strategies in its constructor.
5. **Delegate Work:** The `generate()` method becomes a simple coordinator that calls the formatter then the encryptor.

## 🧠 **Real-World Case Study: "Inheritance vs. Strategy Pattern"**

- **Before (Combinatorial Inheritance):**
  We have `EncryptedPdfReport` and `EncryptedCsvReport`. Both contain nearly identical encryption code because they belong to different branches of the hierarchy.
- **After (Flexible Composition):**

  ```ts
  class Report {
    constructor(
      private formatter: IFormatter,
      private encryptor: IEncryptor,
    ) {}

    public generate(content: string) {
      const fd = this.formatter.format(content);
      return this.encryptor.encrypt(fd);
    }
  }
  ```

- **The Improvement:** We can now create any combination of features at runtime simply by plugging in different "LEGO brick" strategy objects. Adding `XMLExporter` or `RsaEncryption` requires exactly one new class.

## 🤔 **Reflective Questions**

1. How does this design improve compliance with the Open/Closed Principle?
2. What is the "Null Object" pattern, and why is `NoOpEncryptor` a great example of it?
3. What is the core difference between the **Bridge** and **Strategy** patterns?

## 📚 **Further Reading**

- **GoF:** The Strategy, Bridge, and Decorator patterns.
- **Refactoring.Guru:** [Strategy Pattern](https://refactoring.guru/design-patterns/strategy) and [Replace Inheritance with Delegation](https://refactoring.guru/replace-inheritance-with-delegation).

## 📝 **Mini Task (Production)**

Take the "Before" code from this lesson and perform the full refactoring.

1. Implement `IReportFormatter` and `IEncryptionStrategy`.
2. Implement `PdfFormatter`, `CsvFormatter`, `AesEncryptor`, and `NoOpEncryptor`.
3. Build the final `Report` class.
4. Show client code generating:
   - An unencrypted PDF report.
   - An encrypted CSV report.

**Mission:** Prove that the hierarchy is dead and the code is now purely compositional.

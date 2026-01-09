# Lesson 9: Git for Flutter

## Learning Goals

- `.gitignore`.
- Committing code.

## 1. The .gitignore File

Flutter projects generate a lot of build artifacts. You must NOT commit them.
A standard Flutter `.gitignore` includes:

```
/build/
/.dart_tool/
/.idea/
*.iml
.flutter-plugins
.flutter-plugins-dependencies
```

## 2. Best Practices

- **Initial Commit**: Run `git init`, set up `.gitignore`, then `git add .`.
- **Pubspec.lock**: DO commit this. It ensures your team uses exact versions of dependencies.

## Key Takeaways

- Never commit the `build` folder.
- Use branches for features.

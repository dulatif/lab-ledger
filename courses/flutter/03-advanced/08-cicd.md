# Lesson 8: CI/CD

## Learning Goals

- Automation.
- GitHub Actions.

## 1. What is CI/CD?

- **CI (Continuous Integration)**: Run tests on every PR.
- **CD (Continuous Deployment)**: Build IPA/APK and upload to Store.

## 2. GitHub Actions Workflow

Example `.github/workflows/flutter.yml`:

```yaml
name: Flutter Build
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: subosito/flutter-action@v2
      - run: flutter pub get
      - run: flutter test
      - run: flutter build apk
```

## Key Takeaways

- Don't build on your laptop. Build in the cloud.
- It ensures a "clean" build environment.

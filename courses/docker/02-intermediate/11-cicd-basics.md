# 11. CI/CD Integration Basics

## 🎯 Learning Goal

Automating the Build and Push.

## 🧠 Concept

In a pipeline (GitHub Actions, Jenkins):

1.  **Checkout Code**.
2.  **Build**: `docker build -t app .`
3.  **Test**: `docker run app npm test`.
    - This ensures tests run in the _exact_ environment as production.
4.  **Push**: `docker push registry/app`.
5.  **Deploy**: Tell server to pull new image.

## 💻 Implementation: GitHub Actions Snippet

```yaml
jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build Docker Image
        run: docker build -t my-app .
      - name: Run Tests
        run: docker run my-app npm test
      - name: Login to DockerHub
        run: docker login -u ${{ secrets.USER }} -p ${{ secrets.TOKEN }}
      - name: Push
        run: docker push my-app
```

## 🧩 Activity / Challenge

1.  Why run tests in container vs on the Ubuntu Runner directly?
    - Consistency. The Runner might have Node 14. Your app needs Node 18. The Dockerfile ensures Node 18.

## 🔑 Key Takeaways

- The output of CI is a **Container Image**, not a zip file of code.
- This image is an immutable artifact.

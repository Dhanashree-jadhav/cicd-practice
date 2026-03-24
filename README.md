# cicd-practice
# 🚀 CI/CD Workflow Assignment - GitHub Actions

## 📌 Overview

This project demonstrates the implementation of a **CI/CD pipeline using GitHub Actions**.
The workflow automates the process of building, testing, and deploying a Docker image to Docker Hub.

---

## 🎯 Objectives

* Run workflow on **Pull Requests**
* Enable **manual workflow execution**
* Implement **job dependencies (build → test → docker)**
* Use **GitHub context variables**
* Build and push a **Docker image securely**

---

## ⚙️ Workflow Triggers

The workflow is configured to run:

* On **Pull Requests**
* Manually using **workflow_dispatch**

```yaml
on:
  pull_request:
    branches:
      - main
  workflow_dispatch:
```

---

## 🏗️ Job Dependency Design

The pipeline includes three jobs:

### 🔹 Build Job

* Runs on `ubuntu-latest`
* Prints branch & commit info
* Simulates build process

### 🔹 Test Job

* Runs only after build completes
* Uses `needs: build`

### 🔹 Docker Job

* Runs after test completes
* Builds and pushes Docker image

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build Step
        run: echo "Building application..."

  test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Test Step
        run: echo "Running tests..."
```

---

## 🔍 Using GitHub Context Variables

The workflow prints:

* Branch name
* Commit ID

```yaml
- name: Print Branch and Commit
  run: |
    echo "Branch: ${{ github.ref }}"
    echo "Commit ID: ${{ github.sha }}"
```

---

## 🔁 Pull Request Workflow

The workflow automatically triggers on:

* Pull Request creation
* Pull Request updates

### Execution Flow:

👉 Build → Test → Docker

---

## 🐳 Docker Build & Push

The workflow performs:

* Docker login using GitHub Secrets
* Build image using `--build-arg`
* Tag image using environment variables
* Push image to Docker Hub
* Remove local image

```yaml
- name: Docker Build and Push
  env:
    REGISTRY: docker.io
    REPOSITORY: dhanuj16/demo-app
    IMAGE_TAG: latest
  run: |
    docker build --build-arg ENV=dev -t $REGISTRY/$REPOSITORY:$IMAGE_TAG .
    docker push $REGISTRY/$REPOSITORY:$IMAGE_TAG
    docker rmi $REGISTRY/$REPOSITORY:$IMAGE_TAG
```

---

## 🔐 Security

Docker credentials are stored securely using **GitHub Secrets**:

* `DOCKER_USERNAME`
* `DOCKER_PASSWORD` (Access Token recommended)

---

## ✅ Outcome

* CI/CD pipeline successfully implemented
* Workflow triggers on PR and manual execution
* Job dependencies enforced correctly
* Docker image built and pushed to Docker Hub
* Secure authentication using secrets

---

## 🏆 Conclusion

This project demonstrates a **real-world CI/CD pipeline** using GitHub Actions with Docker integration.
It covers essential DevOps practices like automation, dependency management, and secure deployments.

---

## 🚀 Future Improvements

* Use dynamic image tags (`github.sha`) instead of `latest`
* Add real test scripts
* Implement caching for faster builds
* Deploy container to cloud (AWS, Azure, etc.)

---

✨ **Author:** Dhanashree Jadhav
✨ **Domain:** DevOps & Cloud Engineering

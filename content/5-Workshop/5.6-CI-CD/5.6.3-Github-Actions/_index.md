---
title : "Setup GitHub Actions Workflows"
date : 2026-07-30
weight : 3
chapter : false
pre : " <b> 5.6.3 </b> "
---

#### Create Workflow Files
With the AWS infrastructure ready, we will write the YAML workflow files to instruct GitHub Actions on how to build, test, and deploy our code. We will separate the pipelines into **Continuous Integration (CI)** for testing pull requests and **Continuous Deployment (CD)** for deploying merged code.

1. In the root of your local project, create a directory named `.github/workflows/`.
2. Inside this directory, create four files: `backend-ci.yml`, `backend-cd.yml`, `frontend-ci.yml`, and `frontend-cd.yml`.

#### 1. Backend CI Workflow
This workflow runs automated checks (Linter, Unit Tests, and Build) whenever a Pull Request is made to the `main` branch affecting the `backend` directory. 

Create `.github/workflows/backend-ci.yml`:

```yaml
name: Backend CI (NestJS)

on:
  pull_request:
    branches: [ main ]
    paths:
      - 'backend/**'
jobs:
  build-and-test:
    runs-on: ubuntu-latest
    
    defaults:
      run:
        working-directory: ./backend

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: ⚙️ Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '24'
        cache: 'npm'
        cache-dependency-path: ./backend/package-lock.json

    - name: Install dependencies
      run: npm ci

    - name: Run Linter
      run: npm run lint

    - name: Run Unit Tests
      run: npm run test

    - name: Build Application
      run: npm run build

```

Create `.github/workflows/backend-cd.yml`:

```yaml
name: Backend CD (Deploy to AWS ECS)

on:
  push:
    branches: [ main ]
    paths:
      - 'backend/**'

env:
  AWS_REGION: us-east-1
  ECR_REPOSITORY: edushare-backend
  ECS_CLUSTER: edushare-cluster
  ECS_SERVICE: edushare-api-service

jobs:
  deploy:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: ./backend

    steps:
    - name: 📥 Checkout code
      uses: actions/checkout@v4

    - name: 🔐 Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ env.AWS_REGION }}

    - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v2

    - name: 🐳 Build, tag, and push image to Amazon ECR
      env:
        ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
        IMAGE_TAG: latest 
      run: |
        docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG

    - name: 🚀 Force new deployment on Amazon ECS
      run: |
        aws ecs update-service \
          --cluster ${{ env.ECS_CLUSTER }} \
          --service ${{ env.ECS_SERVICE }} \
          --force-new-deployment \
          --region ${{ env.AWS_REGION }}
```

Create `.github/workflows/frontend-ci.yml`:

```yaml
name: Frontend CI (Next.js)

on:
  pull_request:
    branches: [ main ]
    paths:
      - 'frontend/**'

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    
    defaults:
      run:
        working-directory: ./frontend

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '20'
        cache: 'npm'
        cache-dependency-path: ./frontend/package-lock.json

    - name: Install dependencies
      run: npm ci

    - name: Run Linter
      run: npm run lint

    - name: Test Build Application
      run: npm run build
      env:
        NEXT_PUBLIC_API_URL: "http://localhost:3000"
```

Create `.github/workflows/frontend-cd.yml`:

```yaml
name: Frontend CD (Deploy to AWS ECS)

on:
  push:
    branches: [ main ]
    paths:
      - 'frontend/**'

env:
  AWS_REGION: us-east-1
  ECR_REPOSITORY: edushare-frontend
  ECS_CLUSTER: edushare-cluster
  ECS_SERVICE: edushare-frontend-task-service

jobs:
  deploy:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: ./frontend

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ env.AWS_REGION }}

    - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v2

    - name: 🐳 Build, tag, and push image to Amazon ECR
      env:
        ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
        IMAGE_TAG: latest 
      run: |
        docker build --build-arg NEXT_PUBLIC_API_URL=${{ secrets.NEXT_PUBLIC_API_URL }} -t$ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
        
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG

    - name: 🚀 Force new deployment on Amazon ECS
      run: |
        aws ecs update-service \
          --cluster ${{ env.ECS_CLUSTER }} \
          --service ${{ env.ECS_SERVICE }} \
          --force-new-deployment \
          --region ${{ env.AWS_REGION }}
```
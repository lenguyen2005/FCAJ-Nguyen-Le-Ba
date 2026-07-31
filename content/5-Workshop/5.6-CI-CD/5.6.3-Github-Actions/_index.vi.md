---
title : "Thiết lập GitHub Actions Workflows"
date : 2026-07-30
weight : 3
chapter : false
pre : " <b> 5.6.3 </b> "
---

#### Tạo các tệp Workflow
Khi hạ tầng AWS đã sẵn sàng, chúng ta sẽ viết các tệp cấu hình YAML để chỉ định GitHub Actions cách build, test và deploy mã nguồn. Chúng ta sẽ chia pipeline thành hai luồng chính: **Continuous Integration (CI)** để kiểm thử các Pull Request và **Continuous Deployment (CD)** để triển khai mã nguồn đã được merge.

1. Tại thư mục gốc của dự án, tạo một thư mục tên là `.github/workflows/`.
2. Bên trong thư mục này, tạo 4 tệp: `backend-ci.yml`, `backend-cd.yml`, `frontend-ci.yml`, và `frontend-cd.yml`.

#### 1. Luồng Backend CI 
Workflow này sẽ tự động chạy các bước kiểm tra (Linter, Unit Tests và Build) mỗi khi có Pull Request trỏ vào nhánh `main` và có sự thay đổi trong thư mục `backend`.

Tạo tệp `.github/workflows/backend-ci.yml`:

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
        docker build --build-arg NEXT_PUBLIC_API_URL=${{ secrets.NEXT_PUBLIC_API_URL }} -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
        
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG

    - name: 🚀 Force new deployment on Amazon ECS
      run: |
        aws ecs update-service \
          --cluster ${{ env.ECS_CLUSTER }} \
          --service ${{ env.ECS_SERVICE }} \
          --force-new-deployment \
          --region ${{ env.AWS_REGION }}
```
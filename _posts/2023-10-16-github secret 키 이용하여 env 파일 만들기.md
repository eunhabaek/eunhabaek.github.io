---
title: "github secret 키 이용하여 .env 파일 만들기"

categories:
  - CICD
tags:
  - Deployment
last_modified_at: 2023-10-16
---


### github secret 키 이용하여 .env 파일 만드는 코드 추가

    # This is a basic workflow to help you get started with Actions
    
    name: CI-pipeline
    
    # Controls when the workflow will run
    on:
      # Triggers the workflow on push or pull request events but only for the "main" branch
      push:
        branches: [ "deploy" ]
    
    
    permissions:
      id-token: write # This is required for requesting the JWT
      contents: read # This is required for actions/checkout
      packages: write
    
    env:
      AWS_REGION: ${{secrets.AWS_REGION}}
      ECR_REPOSITORY: ${{secrets.AWS_FASTAPI_REPO}}
      ECS_SERVICE: lv3-fastapi-svc
      ECS_CLUSTER: ${{secrets.AWS_CLUSTER}}
      ECS_TASK_DEFINITION: task-definition.json
      CONTAINER_NAME: lv3fastapicontainer
      IMAGE_TAG: ${{ github.sha }}
    
    # A workflow run is made up of one or more jobs that can run sequentially or in parallel
    jobs:
      run-test-code:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v3
          - uses: actions/setup-python@v3
            with:
              python-version: "3.9"
          - run: pip install -r requirements.txt
      build:
        needs: [run-test-code] # 이 코드 실행 후에 빌드 함
        runs-on: ubuntu-latest
        steps:
          - name: Checkout
            uses: actions/checkout@v3
          - name: Set up python 3.9
            uses: actions/setup-python@v3 
            with:
              python-version: "3.9"
          - name: Install Dependencies
            run: |
              python -m pip install --upgrade pip
              pip install -r requirements.txt
    
          - name: Configure AWS Credentials
            uses: aws-actions/configure-aws-credentials@v4
            with:
              role-to-assume: ${{secrets.AWS_ROLE}}
              role-session-name: sampleSessionName
              aws-region: ${{secrets.AWS_REGION}}
    
          - name: Login to Amazon ECR
            id: login-ecr
            uses: aws-actions/amazon-ecr-login@62f4f872db3836360b72999f4b87f1ff13310f3a
                # Github Repository Secrets를 통해 환경 변수 파일을 생성합니다.
            
          - name: Generate Environment Variables File for Production
            run: |
              touch .env
              echo CREDENTIALS_ACCESS_KEY=${{ secrets.CREDENTIALS_ACCESS_KEY }} >> .env
              echo CREDENTIALS_SECRET_KEY=${{ secrets.CREDENTIALS_SECRET_KEY }} >> .env
              echo DB_URL=${{ secrets.DB_URL }} >> .env
              echo DB_NAME=${{ secrets.DB_NAME }} >> .env
              echo COL_NAME=${{ secrets.COL_NAME }} >> .env
              echo OPEN_API_KEY=${{ secrets.OPEN_API_KEY }} >> .env
    
          - name: Setup docker buildx
            id: buildx
            uses: docker/setup-buildx-action@v1
    
          - name: Build, tag, and push image to Amazon ECR
            id: build-image
    
            env:
              ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
              IMAGE_TAG: ${{ github.sha }}
            run: |
              docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
              docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG
              echo "image=$ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG" >>$GITHUB_OUTPUT
          
    
          - name: Fill in the new image ID in the Amazon ECS task definition
            id: task-def
            uses: aws-actions/amazon-ecs-render-task-definition@c804dfbdd57f713b6c079302a4c01db7017a36fc
            with:
              task-definition: ${{ env.ECS_TASK_DEFINITION }}
              container-name: ${{ env.CONTAINER_NAME }}
              image: ${{ steps.build-image.outputs.image }}
          
          - name: Deploy Amazon ECS task definition
            uses: aws-actions/amazon-ecs-deploy-task-definition@df9643053eda01f169e64a0e60233aacca83799a
            with:
              task-definition: ${{ steps.task-def.outputs.task-definition }}
              service: ${{ env.ECS_SERVICE }}
              cluster: ${{ env.ECS_CLUSTER }}
              wait-for-service-stability: true

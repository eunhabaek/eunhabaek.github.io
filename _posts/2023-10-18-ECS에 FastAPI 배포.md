---
title: "ECS에 FastAPI 앱 배포"

categories:
  - AWS
tags:
  - Deployment
last_modified_at: 2023-10-18
---

FastAPI Dockerfile
------------------

*   도커 이미지를 빌드하기 위한 dockerfile
    
        # https://fastapi.tiangolo.com/deployment/docker/
        
        FROM python:3.9-slim-buster
        
        WORKDIR /code
        
        COPY ./requirements.txt /code/requirements.txt
        
        RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt
        
        # 두 줄은 배포할 때 주석 해제하고 실행
        COPY ./app /code/app
        
        CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0","--port", "8000"]
        
        # uvicorn app.main:app --relaod
    

Git action 이용 ECS에 배포 코드
------------------------

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
    
    env:
      AWS_REGION: ${{secrets.AWS_REGION}}
      ECR_REPOSITORY: ${{secrets.AWS_FASTAPI_REPO}}
      ECS_SERVICE: lv3-fastapi-svc
      ECS_CLUSTER: ${{secrets.AWS_CLUSTER}}
      ECS_TASK_DEFINITION: task-definition.json
      CONTAINER_NAME: lv3fastapicontainer
    
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

git action 속도 문제
----------------

*   평균 20분 정도로 굉장히 오래 걸림
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/ed82dec8-2577-40e7-920f-31755fbe01f8)


캐싱 기능 추가
--------

[https://keyhyuk-kim.medium.com/github-actions-에서-도커-캐시-적용하기-6f1f5d5b1f62](https://keyhyuk-kim.medium.com/github-actions-%EC%97%90%EC%84%9C-%EB%8F%84%EC%BB%A4-%EC%BA%90%EC%8B%9C-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0-6f1f5d5b1f62)

    - name: Setup docker buildx
      id: buildx
      uses: docker/setup-buildx-action@v1
    
    - name: Build, tag, and push image to Amazon ECR
      id: build-image
      uses: docker/build-push-action@v2
      env:
        ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
        IMAGE_TAG: ${{ github.sha }}
      with:
        context: .
        push: true
        tags: ${{env.ECR_REGISTRY}}/${{env.ECR_REPOSITORY}}:${{env.IMAGE_TAG}}
        cache-from: type=gha
        cache-to: type=gha,mode=max

*   다음과 같이 캐싱 기능 추가

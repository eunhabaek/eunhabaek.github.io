---
title: "ECS에 SpringBoot 앱 배포"

categories:
  - AWS
tags:
  - Deployment
last_modified_at: 2023-10-20
---

SpringBoot Dockerfile
---------------------

*   Docker 이미지 빌드를 위한 dockerfile
    
        FROM amazoncorretto:17
        CMD ["./mvnw", "clean", "package"]
        ARG JAR_FILE=./build/libs/*.jar
        COPY ${JAR_FILE} app.jar
        ENTRYPOINT ["java", "-jar", "app.jar"]
    

ECS에 배포 위한 git action 코드
------------------------

    
    name: Java CI with Gradle
    
    on:
      push:
        branches: ["deploy"]
    
    permissions:
      contents: read
      id-token: write
    
    env:
      AWS_REGION: ap-northeast-2
      ECR_REPOSITORY: lv3-springboot
    
      ECS_SERVICE: lv3-springboot-svc
    
      ECS_CLUSTER: lv3cluster
      ECS_TASK_DEFINITION: task-definition.json
      CONTAINER_NAME: lv3container
    
    jobs:
      build:
        runs-on: ubuntu-latest
        
        steps:
          - uses: actions/checkout@v3
          - name: Set up JDK 17
            uses: actions/setup-java@v3
            with:
              java-version: '17'
              distribution: 'temurin'
                
          - name: Configure AWS credentials
            uses: aws-actions/configure-aws-credentials@v4
            with:
    
              role-to-assume: arn:aws:iam::682971737440:role/eunhadeploy
              role-session-name: sampleSessionName
              aws-region: ap-northeast-2
    
          - name: Login to Amazon ECR
            id: login-ecr
            uses: aws-actions/amazon-ecr-login@62f4f872db3836360b72999f4b87f1ff13310f3a
    
          - name: Exclude test code
            run : ./gradlew clean build --exclude-task test
    
          - name: Build with Gradle
            uses: gradle/gradle-build-action@67421db6bd0bf253fb4bd25b31ebb98943c375e1
            with:
              arguments: build
    
          - name: Build, tag, and push image to Amazon ECR
            id: build-image
            env:
              ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
              IMAGE_TAG: ${{ github.sha }}
            run: |
              # Build a docker container and
              # push it to ECR so that it can
              # be deployed to ECS.
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


캐싱 코드 추가하여 빌드/배포 속도 줄이기
--------

    - name: Gradle Caching
            uses: actions/cache@v3
            with:
              path: |
                ~/.gradle/caches
                ~/.gradle/wrapper
              key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
              restore-keys: |
                ${{ runner.os }}-gradle-

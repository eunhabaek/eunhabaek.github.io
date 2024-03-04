---
title: "Git action 이용 docker hub 배포 (1)"

categories:
  - CI/CD
last_modified_at: 2023-08-22
---

### git hub action 이용 docker hub 배포 과정

*   동작하는 app과 Dockerfile 작성
    
    *   이미지 생성
        *   docker build -t goapp2 .
    
    *   컨테이너 실행
        
        *   docker run —name goapp2 goapp2
        
        *   계속 동작하는 경우 -dit, 외부 접근하는 경우 -p 포트:포트 작성 필요

*   app과 Dockerfile을 github에 업로드
    
        git init
        git add .
        git commit -m "make goapp2"
        git remote add origin https://github.com/eunhabaek/goapplication.git
        git push origin main
    

*   docker hub에서 access token 발급 받고 (1회) repository 생성
    *   goapp2

*   github에서 git action 생성 (workflows의 yaml파일)
    
        name: GoApplication
        on:
          push:
            branches: [ "main" ] #메인 브랜치에 push 발생시
          pull_request:
            branches: [ "main" ] #메인 브랜치에 pr 발생시
        jobs:
          build:
            runs-on: ubuntu-latest
            steps:
              - uses: actions/checkout@v3
              - name: set up go
                uses: actions/setup-go@v3
                with:
                  go-version: 1.15
        
              - name: Build
                run: go build -v ./...
        
                #여기부턴 정해져있음
              - name: Login To DockerHub
                uses: docker/login-action@v1
                with:
                  username: eunhabaek
                  password:
                  
              - name: build and release to DockerHub
                env:
                  NAME: eunhabaek
                  REPO: goapp2
                run: |
                  docker build -t $REPO:latest .
                  docker tag $REPO:latest $NAME/$REPO:latest
                  docker push $NAME/$REPO:latest
    

*   반영 확인
    
    *   로컬에서 변경 내용 반영
        
            git pull origin main
            git add .
            git commit -m "어쩌구 저쩌구"
            git push origin main
        
    
    *   docker run 수행
        
        *   `docker run eunhabaek/goapp2`
        
        *   main 코드 수행됨

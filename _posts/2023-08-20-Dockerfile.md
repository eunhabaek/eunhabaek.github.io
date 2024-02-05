Dockerfile
==========

node 이용 dockerfile 만들고 실행
-------------------------

*   설치
    
    *   windows는 다운 받아 설치
    
    *   mac은 brew 이용

*   프로젝트 만들기
    
    *   빈 디렉토리 생성
    
    *   npm init
        *   설정 후 yes
    
    *   npm install express
        *   웹 패키지 설치
    
    *   package.json 확인
        
        *   의존성 관리 파일
        
        *   이 파일 있는 상태에서 npn install 명령 치면 어디서든 앱 설치 가능
        
        *   파이썬은 이런 파일 없어서 pip freeze>requirements.txt 명령으로 만들고 pip install - r requirements.txt이용
    
    *   app.js 실행파일 만들기
        
            const express=require('express')
            const app=express()
            
            app.set('port',precess.env.PORT || 3000)
            
            app.get('/', (req,res)=>{
            	res.send('Hello')
            })
            
            app.listen(app.get('port'),()=>{
            	consol.log('대기중')
            }
        
    
    *   package.json 수정
        
            {
              "name": "nodeapp",
              "version": "1.0.0",
              "description": "",
              "main": "index.js",
              "scripts": {
              "start":"node app.js"},
              "author": "",
              "license": "ISC",
              "dependencies": {
                "express": "^4.18.2"
              }
            }
        
    
    *   npm start
    
    *   도커파일 만들기
        
            FROM node:latest
            
            #작업디렉토리 생성
            RUN mkdir -p /app
            
            #작업디렉토리로 이동
            WORKDIR /app
            
            #app 디렉토리에 파일 복사
            ADD . /app
            
            #라이브러리 설치
            RUN npm install
            
            #포트 개방
            EXPOSE 3000 80
            
            #컨테이너에서 실행할 명령 => 웹서버 역할 수행
            CMD ["npm","start"]
        
    
    *   도커 빌드
        *   docker build -t 이미지명 \[-f 도커파일경로\]
    
    *   컨테이너 실행
        *   doxker run -dit -p 3000:3000 —name=nodeapp nodeapp
    
    *   [localhost:3000](http://localhost:3000) 에서 확인
    
    GO 이용 다단계 빌드
    ------------
    
    *   설치
        *   [https://go.dev/dl/](https://go.dev/dl/)
    
    *   Mac은 환경변수 설정
        
        *   export PATH=$PATH:/usr/local/go/bin
        
        *   export PATH=$GOPATH:/
    
    *   go는 클라우드에서 많이 이용
    
    *   수행 과정
        
        *   빈 디렉토리 생성
        
        *   프로그램 생성 main.go
            
                package main
                
                import "fmt"
                
                func main(){
                	fmt.Println('Hello GO")
                }
            
        
        *   빌드 및 실행
            
            *   go build main.go
                
                *   리눅스에서 동작하는 경우
                    *   GOOS=linux GOARCH=amd64 go build main.go
                
                *   이름 변경할 시 -o 원하는이름.exe
            
            *   만들어진 main.exe 실행
                *   main.exe
        
        *   도커파일 만들기
            
                #이미지에 불포함시키는 빌드 작업
                FROM golang:1.15-alpine3.12 AS gobuilder-stage
                
                #작업 디렉토리 설정
                WORKDIR /usr/src/goapp
                
                #실행파일 복사
                COPY main.go .
                
                #빌드 작업
                #도커의 운영체제 시스템을 설정해야 함!
                RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o /usr/local/bin/gostart
                
                
                #이미지에 포함시키는 실행 스테이지
                FROM scratch AS runtime-stage
                
                #복사
                COPY --from=gobuilder-stage /usr/local/bin/gostart /usr/local/bin/gostart
                
                #실행 명령
                CMD ["/usr/local/bin/gostart"]
            
        
        *   이미지 만들기
            *   docker build -t goapp:1.0 . -f ./Dockerfile.txt
        
        *   컨테이너 생성
            *   docker run —name goapp goapp:1.0
    
    ### 명령어 별 실행 과정
    
    java
    
    jvm 이용
    
    node
    
    node app.js
    
    python
    
    python app.py
    
    c/go
    
    빌드 후 바이너리 파일 생성해서 c나 go 없어도 실행 가능
    
    → c나 go는 실행 파일만 가지고 이미지 만들어 용량 줄일 수 있음
    
    → java, node, python 등은 실행 위해 언어나 jvm등을 필요로 함
    
    → 클라우드 환경에서 go 많이 쓰는 이유
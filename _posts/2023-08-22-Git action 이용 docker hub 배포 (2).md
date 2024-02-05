
Git action 이용 docker hub 배포
===========================

mongodb 연동 flask web db 파싱 실습
-----------------------------

*   실습 개요: mongoDB 연동하는 flask web db 만들어 파싱 수행하는 함수 테스트하고 테스트 통과하면 Dockerfile 이용해서 빌드 한 후 배포하는 앱

### MongoDB 컨테이너 실행

*   도커에서 실행
    *   docker run —name mongodb -v ~/data:/data/db -dit -p 27017:27017 mongo

*   bash 접속
    *   docker exec -it mongodb bash

### 가상환경 만들고 접속

*   python -m venv venv

*   cd \\venv\\Scripts

*   activate

### 필요한 패키지 설치

*   flask

*   requests, beautifulsoup4

*   pymongo

### [app.py](http://app.py) 작성

    from flask import Flask, render_template, jsonify, request
    
    app=Flask(__name__)
    
    #mongoDB 준비
    from pymongo import MongoClient
    client=MongoClient('mongo',27017)
    db=client.eunha
    
    @app.route('/')
    def home():
        return render_template('index.html')
    
    if __name__=='__main__':
        app.run('0.0.0.0',port=5000,debug=True)

### templates/index.html 작성

    <body><h1>Hello!!</h1>
        <h1>Hi Flask Web</h1>
        <h1>Hello!!</h1>
        <h1>Hi Flask Web</h1>
        <h1>Hello!!</h1>
        <h1>Hi Flask Web</h1>
        <h1>Hello!!</h1>
        <h1>Hi Flask Web</h1>
        <h1>Hello!!</h1>
        <h1>Hi Flask Web</h1></body>

### 영화정보 파싱 함수

*   [utils.py](http://utils.py) 파일 
    
        import requests
        from bs4 import BeautifulSoup
        
        def get_movie_info(url_receive):
            headers={
                'User-Agent':"mozilla/5.0 (windows nt 10.0; win64; x64) applewebkit/537.36 (khtml, like gecko) chrome/118.0.0.0 safari/537.36"
            }
        #html 읽어오기
            data=requests.get(url_receive,
                            headers=headers)
            soup=BeautifulSoup(data.text, 'html.parser' )
        
            #파싱
            title=soup.select_one('div.title_area._title_area > h2 > span > strong').get_text()
            image=soup.select_one('div.cm_content_area._cm_content_area_info > div > div.detail_info > a > img')['src']
            desc=soup.select_one('div > div.detail_info > div > span').get_text()
        
        	    return title, image, desc 
    

### 테스트 함수 만들기

*   test\_utils.py
    
        from utils import get_movie_info
        
        def test_get_movie_info():
            test_url="https://m.search.naver.com/search.naver?sm=tab_hty.top&where=nexearch&query=%EC%9D%B8%EA%B0%84%2C+%EA%B3%B5%EA%B0%84%2C+%EC%8B%9C%EA%B0%84+%EA%B7%B8%EB%A6%AC%EA%B3%A0+%EC%9D%B8%EA%B0%84&x_csa=%7B%22mv_id%22%3A%22163379%22%7D&pkid=68"
        
            title, image, desc = get_movie_info(test_url)
            print(title)
            print(image)
            print(desc)
        
            assert title =='인간, 공간, 시간 그리고 인간'
    

### [app.py](http://app.py) 수정

    from flask import Flask, render_template, jsonify, request
    from utils import get_movie_info
    
    app=Flask(__name__)
    
    #mongoDB 준비
    from pymongo import MongoClient
    client=MongoClient('127.0.0.1')
    db=client.eunha
    
    @app.route('/')
    def home():
        return render_template('index.html')
    
    @app.route('/memo',methods=['GET'])
    def listing():
        articles=list(db.articles.find({},{'_id':False}))
        return jsonify({'all_articles':articles})
    
    @app.route('/memo',methods=['POST'])
    def saving():
        url_receive=request.form['url_give']
        comment_receive=request.form['comment_give']
    
        title, image, desc=get_movie_info(url_receive)
    
        doc={
            'title':title,
            'image':image,
            'desc':desc,
            'url':url_receive,
            'comment':comment_receive
        }
        db.articles.insert_one(doc)
    
        return jsonify({'msg':"저장이 완료되었습니다!!"})
    
    if __name__=='__main__':
        app.run('0.0.0.0',port=5000,debug=True)

### Web API test

*   GET test: URL로 테스트 가능

*   POST: POSTMAN 등의 도구 이용해야 함

### github 업로드

*   필요한 패키지를 텍스트 파일에 내보내기
    *   pip freeze>requirements.txt

*   git repo에 폴더 올리기

### git action 추가

*   로컬 repo에 .github/workflows 디렉토리 만들어 yaml 추가하거나

*   원격 repo에서 yaml 파일 작성하기
    
        # This is a basic workflow to help you get started with Actions
        
        name: CI-Test
        
        # Controls when the workflow will run
        on:
          # Triggers the workflow on push or pull request events but only for the "main" branch
          push:
            branches: [ "main" ]
          pull_request:
            branches: [ "main" ]
        # A workflow run is made up of one or more jobs that can run sequentially or in parallel
        jobs:
          run-test-code:
            runs-on: ubuntu-latest
            steps:
             - uses: actions/checkout@v3
             - uses: actions/setup-python@v2 # 언어에 따라 바뀌는 부분
               with:
                 python-version: "3.8"
             - run: pip install -r requirements.txt
             - run: pytest
    

### 컨테이너로 실행

*   도커 파일 추가
    *   보통 루트에 작성
        
            FROM python:3.8-slim
            
            RUN apt-get update
            
            #필요한 패키지들 설치 (대화형 작업은 못 하므로 -y 필수)
            RUN apt-get install -y --no-install-recommends
            
            #작업 디렉토리 만들기
            WORKDIR /usr/src/app
            
            #필요한 파일 복사
            COPY requirements.txt ./
            
            RUN pip install -r requirements.txt
            
            #소스 파일 모두 복사
            COPY . .
            
            #포트 열어주기(app.py에서 확인 가능)
            EXPOSE 5000
            
            #명령어
            CMD ["python","-m","flask","run","--host=0.0.0.0"]
        

*   도커 빌드로 이미지 생성 확인
    *   docker build -t eunhabaek/flaskweb .

*   도커 런 실행해서 컨테이너 생성
    *   docker run -dit -p 5000:5000 --name flaskweb eunhabaek/flaskweb

### Git action 이용해서 docker hub 에 배포

*   git hub에서도 작업 수행했으므로 변경사항 맞추기
    
    *   git pull origin main
    
    *   git add
    
    *   git commit
    
    *   git push origin main

*   docker hub 접속 해서 access 토큰 받고 repository 생성하기
    
    *   token: dckr\_pat\_GGioWjyPD07OBKO068R\_M\_YVf8Y
    
    *   repository명: flaskweb

*   git action 수정(.github/workflows/python-app/yml 수정)
    
        # This is a basic workflow to help you get started with Actions
        
        name: CI-Test
        
        # Controls when the workflow will run
        on:
          # Triggers the workflow on push or pull request events but only for the "main" branch
          push:
            branches: [ "main" ]
          pull_request:
            branches: [ "main" ]
        # A workflow run is made up of one or more jobs that can run sequentially or in parallel
        jobs:
          run-test-code:
            runs-on: ubuntu-latest
            steps:
             - uses: actions/checkout@v3
             - uses: actions/setup-python@v2 #python 3.8은 v2
               with:
                 python-version: "3.8"
             - run: pip install -r requirements.txt
             - run: pytest
          build:
            needs: [run-test-code] # 이 코드 실행 후에 빌드 함
            runs-on: ubuntu-latest
            steps:
              - name: Checkout
                uses: actions/checkout@v3
              - name: Set up python 3.10
                uses: actions/setup-python@v4 #python 3.10은 v4
                with:
                  python-version: "3.10"
              - name: Install Dependencies
                run:
                  python -m pip install --upgrade pip
                  pip install -r requirements.txt
              - name: Login to Docker
                with:
                  username: eunhabaek 
                  password: dckr_pat_GGioWjyPD07OBKO068R_M_YVf8Y
              - name: build and release to DockerHub
                env:
                  NAME: eunhabaek
                  REPO: flaskweb
                run: | 
                  docker build -t $REPO .
                  docker tag $REPO:latest $NAME/$REPO:latest
                  docker push $NAME/$REPO:latest
    

### secret 만들기

*   github settings >secret and variables>actions>
    *   도커 토큰 입력

*   git action yml 수정
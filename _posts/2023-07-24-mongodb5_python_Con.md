---
title: "MongoDB와 python 연동"
excerpt: "MongoDB와 파이썬 연동하기"

categories:
  - database
tags:
  - MongoDB
last_modified_at: 2023-07-24
---
## python mongo DB 연동

- pyCharm에서 new project 생성
    
    ![Untitled](/figures/mongo3.png)
    
- pymongo 설치
    1. pip로 다운
    
    ```bash
    #terminal에서 수행
    pip install pymongo
    ```
    
    1. file>settings> “python interpreter” 검색 후 필요한 프로그램 설치 (pymongo)

## 데이터베이스 연결 객체 생성

- 필요 정보 : DB서버, 포트번호, 계정, 비밀번호 (MongoDB는로컬에서 계정과 비밀번호 없어도 됨)
    
    ```python
    #mongodb 연결 패키지 import
    from pymongo import MongoClient
    
    #연결 객체 생성
    connect=MongoClient('localhost', port=27107)
    ```
    

## 데이터베이스 연결

- 컬렉션 연결
    
    ```python
    #데이터베이스 연결 객체, 없으면 새로 생성 됨
    db=connect.table1
    
    #collection 연결 객체
    coll=db.coll1
    
    #객체 정보확인
    print(dir(coll))
    ```
    
- 컬렉션에 CRUD 수행
    - MongoDB는 프로그래밍 언어와 연동 시 함수명의 변화가 거의 없음
    - 삽입 삭제, 수정
        - 수행 결과 리턴하므로 수행 후 리턴 객체 확인 시 성공 여부 확인가능
            
            ```python
            #데이터 삽입
            #삽입, 삭제, 갱신은 결과를 리턴함
            #데이터 한개 삽입
            result1=coll.insert_one({"name":"은하","나이":27})
            
            #삽입 결과 확인
            print(result1)
            
            #데이터 여러개 삽입
            result2=coll.insert_many([{"name":"철수","나이":35},{"name":"짱구","나이":5}])
            
            #삽입 결과 확인
            print(result1)
            ```
            
- 컬렉션에 조회 수행
    - 조회 결과는 cursor  리턴하므로 for 이용하여 출력
        
        ```python
        #조회 결과 커서 for문으로 확인
        #cursor 순서대로 접근하면 데이터 dict로 접근 가능
        
        for temp in result3:
            print(temp.get("name","no name"))
        ```
        
    - 조건 조회
        
        ```python
        #age가 20 이상인 데이터만 조회하고, age로 오름차순
        
        result4=coll.find({"age":{"$gt":20}}).sort("age")
        
        for temp in result4:
            print(temp.get("name","no name"))
        ```
        
    
    ## MongoDB의 수정
    
    - update, update_many
    - 매개변수 2개
        - **$set** 연산자 이용
        - 첫번쨰는 수정된 데이터의 조건, 두번째는 새 데이터
            
            ```python
            #name이 eunha인 데이터를 baekeunha로 수정
            coll.update_many({"name":"eunha"},{"$set":{"name":"baekeunha"}})
            ```
            
    
    ## MongoDB의 삭제
    
    - delete_one, delete_many
    - 매개변수 1개
    
    ## 도커 이미지 저장
    
    - 터미널에서 작업
        
        ```bash
        docker save -o mongo0723.tar mongo:latest
        ```
        
    
    ## 도커 이미지 다운 및 재설치
    
    - 다른 서버에서 동일한 환경 구축 가능
    - 터미널에서 작업
        
        ```bash
        #이미지 다시 로딩
        docker load -i mongo0723.tar
        
        #도커 컨테이너 재설치
        docker run --name mongodb_eunha -v ~/data:/data/db -d -p 27017:27017 mongo
        ```
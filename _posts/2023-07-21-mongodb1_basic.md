---
title: "MongoDB 시작하기"
excerpt: "MongoDB Docker에 설치 및 접속하기"

categories:
  - database
tags:
  - MongoDB
last_modified_at: 2023-07-21
---

## MongoDB

> 크로스 플랫폼 도큐먼트 지향 NoSQL 데이터베이스 시스템
> 
- 개발자는 저장 프로시저 대신에 자바스크립트 함수 와 유사한 형태의 함수를 만들어서 서버에서 사용할 수 있기 때문에 친숙하고 편리함
- 스키마 생성 없이 데이터 저장 가능
- Mongo Shell은 명령어 입력 터미널인 자바스크립트 인터프리터를 제공하고 내부에서는 C++ 언어로 동작

## Docker에 MongoDB 설치

- cmd 이용 설치
    
    ```bash
    # 도커 실행 후 명령어 실행
    docker run --name mongodb_eunha -v ~/data:/data/db -d -p 27017:27017 mongo
    ```
    

## MongoDB JSON Document - Binary JSON

> MongoDB의 데이터 표현법-Javascript 객체 표현법과 동일
> 
- 장점
    1. **가벼움**
        - 각 필드의 데이터 타입과 길이 먼저 저장(정적)되기 때문에 빠르게 저장 됨
        - 기본데이터 타입으로 C언어의 Primitive  타입(데이터의 참조가 아닌 실제 값)을 사용하기 때문에 빠르게 인코딩
    2. **다양한 자료형을 지원**
        - Objectid: 데이터 저장 시 데이터 구별 위해 MongoDB에서 지정해주는 자료형

## MongoDB의 데이터 표현

### 1. 객체

```jsx
{"key1":"value1","key2":"valkue2"}

```

- key는 중복 불가
- key 순서 다르면 다른 데이터
- key에 공백, “.”, ”$” 포함 불가능
- key는 “_”로 시작 불가 (예약어 중복 방지)
- key는 대소문자 구분

### 2. 배열

```jsx
[데이터 나열]
```

- MongoDB에서는 하나의 데이터를 Document라고 함

### 3. Collection

> MongoDB 도큐먼트의 집합
> 
- MongoDB는 **스키마 만들지 않으므로** 어떤 종류든 삽입 가능
- 실행 속도을 고려하여 데이터를 분할 저장 지향
- 동일 종류끼리 하나의 collection에 저장 지향

## MongoDB의 작업 단위

> Database → Collections → Document
> 

## 데이터베이스 작업 명령어

### 데이터베이스 목록 확인

```jsx
show dbs
```

### 데이터베이스 설정

```jsx
/* 없는 이름 생성시 데이터 생성*/
use 데이터베이스명
```

### 현재 사용 데이터베이스 삭제

```jsx
db.dropDatabase()
```

### 현재 사용중 데이터베이스 확인

```bash
db
```

### 데이터 삽입

```bash
db.mycollection.insertOne({name:1})
```

## 데이터베이스 접속 방법

❕MongoDB는 아이디, 암호가 없고 포트 번호로 접속 가능    

### 1. 설치된 셸에서 직접 접속

- cmd에서 컨테이너 셸 접속
    
    ```bash
    docker exec -it mongodb_eunha bash
    # 프롬프트 #으로 끝남
    ```
    
- bin 디렉토리 이동
    
    ```bash
    cd bin
    ```
    
- mongo shell 실행 → 이제 터미널사용 가능
    
    ```bash
    mongosh
    ```
    

### 2. 접속 프로그램 이용

- robo 3t
    - https://studio3t.com/download-thank-you/?OS=win64
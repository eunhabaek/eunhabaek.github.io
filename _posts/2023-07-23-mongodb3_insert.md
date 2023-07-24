---
title: "MongoDB의 CRUD (1)"
excerpt: "MongoDB의 insertion 관련 명령어"

categories:
  - database
tags:
  - MongoDB
last_modified_at: 2023-07-23
---

## Document 생성

- 특징
    - 하나의 도큐먼트는 하나의 collection에 삽입
    - 데이터 삽입 시\ _id라는 속성에 값 설정하지 않으면 자동으로 만들고 기본 키 값이 됨
    - NoSQL의 collections는 RBDMS의 table과 유사
    - 스키마
        - NoSQL는 일반적으로 dict → create 먼저 하지 않아도 됨
        - RBDMS는 일반적으로 Class

## 작업 실행 단위

### Process

- 실행중인 프로그램, 실행 중에 다른 프로세스 수행 불가

### Thread

- 프로세스 안에서 **독립적으로 동작**하는 작업단위
- 실행 중간에 다른 스레드의 작업 수행  가능하나, 데이터 공유는 통신 또는 공유 메모리(전역) 이용
- 프로세스 간 속도차이로  작업 가능

### Coroutine

- thread에서 빈번한 **Context switching 을 보완**
- 경량화된 thread이며 channel(공유 메모리⇒ 전역X) 이용하여 통신 가능
- 구독과 게시 시스템(ex. 공장 자동화, CQRS 등)이나 병렬처리 구현이 쉬워짐

## 삽입 함수

### 1. Insert(객체[,ordered])

- \_id라는 기본키 컬럼을 가짐, 자료형은 Object_ID
- 동일한 \_id 사용 시 에러 발생
- 삽입 시 배열 대입은 분할해서 수행
    
    ```bash
    db.컬랙션명.insert([{name:"eunha",age:27},{name:"crystal",age:25}])
    ```
    
- 최상위 레벨은 배열 지원하지 않음 → 객체 지향
- insert 함수는 두번째 매개변수 ordered 생략 가능 → thread 옵션 지정 (default는 single-thread)
- 배열 대입 시 single-thread는 오류 발생 시 중단됨, multi-thread는 다른 배열 작업은 실행됨
    
    ```bash
    #고유 인덱스 생성
    db.컬랙션명.createIndex({컬럼명:정렬조건},{unique:true})
    
    #삽입1
    db.컬랙션명.insert({name:"lee"})
    
    #삽입2 -> lee는 이미 있으므로 에러 발생, lee와 park 모두 삽입 에러 
    db.컬랙션명.insert([{name:"lee"},{name:"park"}])
    
    #삽입3 -> multi-thread 옵션 실행 시 lee는 에러 나지만 choi는 정상 삽입
    db.컬랙션명.insert([{name:"lee"},{name:"choi"}],{ordered:false})
    ```
    
- deprecated된 명령어 (insertOne)를 사용하는 것을 지향
    
    ```bash
    db.컬랙션명.insert(객체명)
    #deprecated warning 나옴
    ```
    

### 2. save(객체)

- 동일 \_id 사용 시 수정함
    
    ```bash
    db.컬랙션명.save(객체명)
    ```
    

### 3. insertOne(객체, 옵션(writeConcern …))

- 데이터 1개 삽입을 위해 새로 만들어진 API
- 2개의 매개변수 가지며, 삽입된 데이터의 ObjectID를 리턴
    
    ```bash
    #insert와 동일하게 사용
    db.컬랙션명.insertOne({name:"seoyeon",age:24})
    ```
    
- **writeConcern 옵션**:
    - 데이터 삽입 시 안정성을 위한 옵션
    - 데이터 원본에 완전히 삽입 전에 클라이언트들이 읽지 못하게 함
    - **mongoDB의 느슨한 트랜잭션**을 제어하기 위해 사용
        - 데이터 삽입 시 메모리에 데이터 삽입하고 작업이 많지 않은 시간에 실제 DB로 복사
        - 커밋 사용하지 않음
        - 비정상 종료시 데이터 제대로 삽입되지 않음
            
            ![Untitled]("/figures/mongo1.png")
            

### 4. insertMany(객체)

- 여러개 데이터 삽입, 매개변수 3개
- insertOne에 ordered 옵션 추가
- 자바스크립트 반복문 사용하여 동일하게 구현 가능
    
    ```bash
    var num=1
    for(var i=0;i<3;i++){
    db.table1.insertOne({name:"name"+i,number:num})
    }
    ```
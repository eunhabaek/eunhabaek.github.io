---
title: "MySQL TCL"
excerpt: "MySQL의 트랜잭션"

categories:
  - database
tags:
  - MySQL
last_modified_at: 2023-07-20
---


## Transaction이란?

> 논리적으로 한 번에 이루어져야 하는 작업의 단위
>

## Transaction의 특징
1. **Atomicity**: All or Nothing
2. **Consistency**: 트랜잭션 전 후의 결과는 일관됨
3. **Isolation**: 트랜잭션 작업 중 다른 트랜잭션 간섭 불가
    - Rock 걸림, 한 테이블 동시 수정 불가
4. **Durability**: 완료된 트랜잭션은 변하지 않음


**JSON 트랜젝션 활용 -> JWT**
    - [https://velog.io/@vamos_eon/JWT란-무엇인가-그리고-어떻게-사용하는가-1](https://velog.io/@vamos_eon/JWT%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EA%B0%80-1)

## Transaction 관련 명령어

- **COMMIT**: 현재까지 작업 내역 원본에 반영
- **ROLLBACK** [TO  세이브포인트명] : 생성 지점 혹은 세이브포인트로 이동
- **SAVEPOINT** 세이브포인트명: 세이브포인트 생성
    
    ```sql
    -- DEPT 테이블에 데이터 1개 삽입: 트랜잭션 생성
    INSERT INTO DEPT(DEPTNO,DNAME,LOC) VALUES(60, '회계', '서울');
    SAVEPOINT SV1;
    
    -- DEPT 테이블에 데이터 1개 삽입
    INSERT INTO DEPT(DEPTNO,DNAME,LOC) VALUES(70, '회계', '서울');
    SAVEPOINT SV2;
    
    -- 세이브포인트 SV1으로 이동, SV1으로 이동 시 SV2로는 이동 불가
    ROLLBACK TO SV1;
    
    -- 커밋으로 결과 반영
    COMMIT
    ```
    

## Transaction 생성 시점

- 트랜잭션 없는 시점에 DML 처음 실행 시

## Transaction 종료 시점

- commit이나 rollback 수행 시 자동 종료

## COMMIT 방법

1. 명시적으로 COMMIT 명령 수행
2. AUTO COMMIT
    - DDL이나 DCL 수행
        - 일반적으로 관리자 명령어이므로 바로 반영
            
            ```sql
            -- DEPT 테이블에 데이터 1개 삽입: 트랜잭션 생성
            INSERT INTO DEPT(DEPTNO,DNAME,LOC) VALUES(50, '회계', '서울');
            
            -- DEPT 복사된 DEPTCOPY 생성
            CREATE TABLE DEPTCOPY
            AS SELECT * FROM DEPT;
            
            -- ROLLBACK 해도 이미 DDL(CREATE)로 자동 COMMIT되어서 복구되지 못함 
            ROLLBACK; 
            ```
            
    - 접속 프로그램 정상 종료할 시 자동 COMMIT

## ROLLBACK 방법

- 명시적으로 ROLLBACK 수행
    
    ```sql
    -- DEPT 테이블에 데이터 1개 삽입: 트랜잭션 생성
    INSERT INTO DEPT(DEPTNO,DNAME,LOC) VALUES(50, '회계', '서울');
    
    -- ROLLBACK 수행하여 트랜잭션 시작(데이터 삽입) 전으로 복구
    ROLLBACK;
    ```
    
- 접속 프로그램 비정상 종료하면 ROLLBACK

## Transaction 모드

- **Manual commit**
    - 트랜잭션 직접 조작
- **Auto commit**
    - 하나의 SQL 문장 수행 시 바로 commit
---
title: "MySQL 접속하기"
excerpt: "Docker 에서 MySQL 실행하기"

categories:
  - database
tags:
  - MySQL
last_modified_at: 2023-07-17
---

## MySQL  접속하기

### 1. docker에서 container 생성 후 runnning
    
```bash
docker run --name mysql -dit -e MYSQL_ROOT_PASSWORD=beh1016 -p 3306:3306  mysql
```

### 2. dbeaver 접속 → 파일 새로 만들기 → dbeaver 선택 → 데이터베이스 연결 → MySQL 선택
    
![figure1](/figures/mysql1.png)
    
### 3. 접속 정보 작성
- serverhost=ip 정보 혹은 localhost
- port 기본 설정은 3306
- username, password

![figure2](/figures/mysql2.png)   

### 4. 처음 연결 시 드라이버 다운 필요
- 드라이버 직접 다운 받아서 설정 가능

### 5. SQL 편집기 새로 생성하여 명령어 작성 가능

## SQL 기본 명령어
- 데이터베이스 목록 보기
    
    ```sql
    show databaes;
    ```
    
- 데이터베이스 생성
    
    ```sql
    creat database db명;
    ```
    
- 데이터베이스 삭제
    
    ```sql
    drop database db명;
    ```
    
- 데이터베이스 사용하기
    
    ```sql
    use db명;
    ```
    
- 데이터베이스 테이블 목록 보기
    
    ```sql
    show tables;
    ```
    
- 테이블 구조 확인 명령어 
    
    ```sql
    DESC 테이블명;
    ```

## SQL 작성 규칙

- 대소문자 구분
    - 예약어는 대소문자 구분하지 않음
    - 테이블명에서는 대소문자 구분
    - 컬럼명은 대소문자 구분하지 않음
- 명령문 끝은 ;로 종료

## SQL 종류

- DQL(데이터 쿼리언어, 조회)
    - SELECT
- DDL(데이터 정의어)
    - CREATE, ALTER, DROP, TRUNCATE, RENAME
- DML(데이터 조작어)
    - INSERT, UPDATE, DELETE
- TCL(트랜젝션 조작어)
    - COMMIT, ROLLBACK, SAVEPOINT
- DCL(데이터 제어어)
    - GRANT, REVOKE
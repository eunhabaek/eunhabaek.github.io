---
title: "MySQL DDL (1) "
excerpt: "MySQL의 테이블 생성 및 수정"

categories:
  - database
tags:
  - MySQL
last_modified_at: 2023-07-19
---
## 테이블 관련 명령

### 1. 테이블 생성 구문

- CREATE [TEMPORARY] TABLE [IF NOT EXISTS] 이름(
    컬럼명1 자료형 [컬럼제약조건 나열],  
    컬럼명2 자료형 [컬럼제약조건 나열],  
    …  
    [CONSTRAINT 제약조건명]테이블 제약 조건 나열);  


    ```sql
    -- 테이블 생성
    -- 테이블 이름은 TABLE1
    -- 테이블 속성
    -- NUM 정수, 일련번호, 기본키
    -- NAME은 한글 7자까지 저장하고 글자 수는 변경 안 됨
    -- ADDRESS는 한글 100자까지 저장, 글자 변경 자주 됨
    -- TEL은 숫자로 된 문자열 11자리, 글자 수 변경 안 됨
    -- EMAIL은 영문 100자 이내, 글자 변경 됨
    -- BIRTHDAY는 날짜만 저장
    -- 조회를 주로, 일련번호는 1부터, 인코딩 UTF8
    
    CREATE TABLE TABLE1(
    	NUM INTEGER AUTO_INCREMENT PRIMARY KEY,
    	NAME VARCHAR(21),
    	ADDRESS TEXT(300),
    	TEL VARCHAR(11),
    	EMAIL CHAR(100),
    	BIRTHDAY DATE
    )ENGINE=MyISAM AUTO_INCREMENT=1 DEFAULT CHARSET=UTF8;
    ```
    

### 2. 테이블의 자료형

- 숫자: **INT**(정수), **FLOAT**, **DOUBLE**, **DECIMAL**(전체 자리수[,소수자리수])
- 문자:
    - **CHAR**(바이트 수), **VARCHAR**(바이트 수), **BINARY**(바이트 수), **VARBINARY**(바이트 수)
        - CHAR는 고정 공간, VARCHAR는 가변 공간에 저장→남는 공간 이용 가능
        - VARCHAR의 경우 데이터 변경되어 바이트 수 변경되면 Row Migration 발생
        - 프로그래밍에서 CHAR 연결 시 공백 주의
        - MySQL에서 VARCHAR는 대소문자 구별 하지 않고 비교, 저장할 땐 구별 → BINARY, VARBINARY 사용하여 구별 가능
    - **TEXT, LONGTEXT**
        - TEXT(65535), LONGTEXT(43억글자)는 긴 물자 저장에 이용, 인덱스 불가
    - **BLOB, LONGBLOB**
        - 대용량의 바이트 배열 저장에 사용, 파일 내용 작성에 사용
        - 파일 저장 방식은 경로 저장과 직접 저장 방식이 있음
- 날짜와 시간
    - **DATE**: 날짜만 저장
    - **DATETIME**: 날짜와 시간(초 단위) 저장
    - **TIMESTAMP**: 날짜와 시간 정확히 저장
    - **TIME**: 시간만 저장
    - **YEAR**: 년도만 저장
- 기타
    - **BOOL**: TRUE /FALSE
    - **JSON**: JSON 문자열
    - **GEOMETRY**: 공간 데이터
    

### 3. 테이블 생성 옵션

- **ENGIN**
    - MyISAM: 조회에 유리, 인덱스 기능 우수
    - InnoDB: 트랜잭션 처리(삽입, 삭제, 갱신)에 유리, 인덱스 기능 떨어짐
- **AUTO_INCREMENT**
    - 시퀀스(일련번호)의 초기값 설정
    - ATLER TABLE 테이블명 AUTO_INCREMENT=초기값 이용하여 변경
- **DEFAULT CHARSET**
    - 인코딩 방식으로 , UTF8이나 UTF8mb4 주로 사용
- **COLLATION**
    - 문자 정렬 방식
    

### 4. 테이블 컬럼 추가 : ADD

- ALTER TABLE 테이블명
    
    **ADD** 컬럼명 자료형 [제약 조건];  
    
    ```sql
    -- 테이블의 AGE컬럼을 정수 자료형으로 추가
    ALTER TABLE TABLE1
    ADD AGE INTEGER;
    ```
    
- 이미 컬럼 있는 경우는 NULL 삽입

### 5. 테이블 컬럼 삭제: DROP

- ALTER TABLE 테이블명
    
    **DROP** 컬럼명 자료형 [제약 조건];  
    
    ```sql
    -- 테이블의 AGE컬럼을 삭제
    ALTER TABLE TABLE1
    DROP AGE;
    ```
    
- 기존 데이터 삭제하면 복구 불가

### 6. 테이블 컬럼 수정

1. **CHANGE**: 이름과 자료형 수정
    - ALTER TABLE 테이블명
        
        **CHANGE** 기존 컬럼명 새로운 컬럼명 자료형;  
        
        ```sql
        -- TEL 컬럼명을 PHONE으로 바꾸고 자료형을 정수형으로 변경
        ALTER TABLE TABLE1
        CHANGE TEL PHONE INTEGER;
        ```
        
2. **MOFIDY**: 자료형만 또는 순서 수정
    - 자료형 수정
        - ALTER TABLE 테이블명
            **MOFIDY** 기존 컬럼명 새로운 컬럼명;  
            
    - 컬럼 순서 조정
        - ALTER TABLE 테이블명
            **MODIFY** COLUMN  컬럼명 자료형 FIRST;  
            
        - ALTER TABLE 테이블명
            **MODIFY** COLUMN  컬럼명1 자료형 AFTER  컬럼명2;   
            

    ⚠️ NULL 제약 조건 변경은 자료형 변경  


### 7. 테이블명 변경: RENAME 
- ALTER TABLE 기존 테이블명
    **RENAME** 새로운 테이블명;  
        
### 8. 테이블 삭제: DROP
- **DROP** TABLE 테이블명;
    - 외래키 갖는 테이블은 삭제 불가
        
        ```sql
        -- 테이블 삭제 -> 외래키 있으면 불가능
        DROP TABLE TABLE1;
        
        -- 삭제된 지 확인
        SHOW TABLES;
        ```
            
### 9. 테이블 모든 데이터 삭제: TRUNCATE 
- 복구되지 않으므로 주의❗

### 10. 그 밖의 기능
- **ROW_FORMAT=COMPRESSED 옵션**: 테이블 압축 기능
    - CREATE TABLE의 옵션으로 사용
    - 저장용량 줄어드나 작업 시간 길어짐
- **COMMENT ON**: 테이블의 주석
    - COMMENT ON TABLE 테이블명 IS ‘주석내용’

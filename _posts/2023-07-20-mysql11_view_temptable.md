---
title: "MySQL View, Temporary table"
excerpt: "MySQL의 뷰와 임시 테이블"

categories:
  - database
tags:
  - MySQL
last_modified_at: 2023-07-20
---

## VIEW란?

> **가상의 테이블**
> 

물리적으로 존재하지 않으나 테이블처럼 사용 가능

- 목적
    - **속도**: VIEW 생성 구문은 메모리에 적재하므로 빠름
    - **보안**: 사용자에게 VIEW 제공 시 사용자는 테이블 구조 알 필요 없음
    

## VIEW의 종류

### 1. INLINE VIEW

> FROM 절에서 작성한 서브 쿼리
> 

하나의 SELECT 구문은 테이블 구조의 RESULT SET  리턴

```sql
-- INLINE VIEW
SELECT * FROM (SELECT 절) 이름; 
```

- SQL 구문이 복잡해질 때 메모리 부담을 줄이기 위해서 사용
    
    ```sql
    -- 조인으로 인해 많은 데이터 저장 됨
    SELECT *
    FROM EMP,DEPT
    WHERE EMP.DEPTNO =DEPT.DEPTNO AND JOB='CLERK';
    
    -- 인라인 뷰로 미리 필터링하여 메모리 부담 줄일 수 있음
    SELECT *
    FROM (SELECT * FROM EMP WHERE JOB='CLERK') TEMP ,DEPT
    WHERE TEMP.DEPTNO =DEPT.DEPTNO;
    ```
    

### 2. VIEW

> CREATE VIEW 로 생성된 일반적인 VIEW
> 

- 형식:
    
    ```sql
    CREATE [OR REPLACE] VIEW 뷰이름  
    AS 
    SELECT 구문   
    [WITH CHECK OPTION]  
    [WITH READ ONLY]
    ```
    
- MySQL 버전에 따라 OR REPLACE 지원하지 않는 경우 있음
- **WITH CHECK OPTION**
    - VIEW로 DML 작업 할 때 SELECT 구문에서 조회된 데이터만 가능하도록 하는 옵션, 데이터베이스에 직접 접속했을 때만 가능→ 데이터베이스에서 제공하지 않는 접속도구 사용 시 수행 불가
- **WITH READ ONLY**
    - 읽기 전용의 뷰를 생성
    - DML 불가
- 특징
    - VIEW는 SELECT 구문 가지고 있다가 호출 시 SELECT 구문 수행, 결과 리턴
        
        ```sql
        -- 일반 VIEW 만들기 
        CREATE VIEW SAMPLE
        AS
        SELECT EMPNO,SAL,ENAME
        FROM EMP;
        
        -- VIEW를 테이블처럼 사용 가능
        SELECT *
        FROM SAMPLE;
        ```
        
    - VIEW에서 DML 작업 불가능한 경우
        - 2개 이상의 테이블로 생성하거나 NOT NULL 컬럼있을 시 DML 불가
    - VIEW에서 DML 작업 시 원본이 수정 됨
        
        ```sql
        -- VIEW에 데이터 삽입 -> 원본 테이블과 VIEW에 둘 다 데이터 삽입 됨
        INSERT INTO SAMPLE() VALUES(7777,5000,'EUNHA');
        ```
        
    - VIEW는 구조 변경 불가 → ALTER 불가
- VIEW 삭제
    - DROP VIEW 뷰이름;


## **TEMPORARY** 테이블이란?

> 일시적으로 테이블을 만들어서 사용하는 것, 현재 세션 내에서만 접근이 가능하고 세션이 종료되면 소멸
> 

💡 개발 과정에서 많이 사용  

### 1. TEMPORARY 테이블 생성

- CREATE TEMPORARY TABLE 테이블명(내용);
    - 기존 존재하는 테이블과 중복된 이름 지양
        
        ```sql
        -- 임시 테이블 생상
        CREATE TEMPORARY TABLE TEMPTB(
        	NAME CHAR(20)
        );
        ```
        

### 2. CTE(Common table expressions)

- 쿼리 실행 중에 메모리에 존재하는 테이블
- SQL 작업 중간에 임시 결과를 저장하기 위한 용도
    - 임시테이블은 세션 종료시까지존재하므로 내용 변경하려면 삭제하고 다시 만들어야 함
    - 뷰는 실제 데이터를 가지는 형태이기 때문에 속도가느림
- 기본 형식
    - WITH 테이블명(컬럼명 나열) AS (SELECT 구문)
        - 컬럼명 나열 생략 시 SELECT 구문의 컬럼명 그대로 사용
- SELECT  구문이 복잡한 경우, 동일한 SELECT 구문 여러번 입력하는 경우에 편리하게 사용
    
    ```sql
    -- CTE: SQL 수행 중에만 일시적으로 메모리 공간 할당 받아 사용하는 테이블
    -- CTE 구문은 가장 먼저 수행 됨
    -- 인라인 뷰로 수행 가능해 보이나, 인라인 뷰는 서브 쿼리보다 늦게 수행되므로 불가능
    WITH TEMP AS 
    (SELECT NAME, SALARY,SCORE FROM tStaff WHERE DEPART='영업부' AND GENDER='남')
    
    SELECT *
    FROM TEMP
    WHERE SALARY>=(SELECT AVG(SALARY) FROM TEMP);
    ```
    
- 2개 이상의 CTE 사용하고자 하는 경우 “,”로 나열
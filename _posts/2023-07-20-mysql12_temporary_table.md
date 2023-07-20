---
title: "MySQL Temporary table"
excerpt: "MySQL의 임시 테이블"

categories:
  - database
tags:
  - MySQL
last_modified_at: 2023-07-20
---
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
---
title: "Oracle의 Pseudo column과 Fetch-Offset"
excerpt: "오라클의  Pseudo column과 Fetch-Offset"

categories:
  - database
tags:
  - Oracle
last_modified_at: 2023-07-21
---

## Pseudo column이란?

> 실제 테이블에는 존재하지 않지만 데이터베이스에서 자동으로 만들어주는 컬럼
> 

### **ROWID**

- 데이터의 실제적인 참조 값이며 내부적 구분 유일KEY
- 오라클에서는 인스턴스 번호, 파일 번호, 블록 번호, 블록 내 행 번호로 구성 → 데이터의 위치 확인 가능
- 이 값으로 조회 시 가장 빠름
- SELECT로 조회 가능
    
    ```sql
    -- ROWID 조회하기
    SELECT ROWID, ENAME 
    FROM EMP;
    ```
    
- 중복 제거에 유용
    - 여러 컬럼 조회 중 중복 제거하여 출력하는 경우
        
        ```sql
        -- EMP 테이블에서 DEPTNO 별로 한 명의 DEPTNO 와 ENAME 조회
        -- DISTINCT, GROUP BY 둘 다 불가
        SELECT ENAME, DEPTNO 
        FROM EMP
        WHERE ROWID IN (SELECT MAX(ROWID) FROM EMP GROUP BY DEPTNO);
        ```
        

### **ROWNUM**

- SELECT 구문 결과로 만들어진 행 일련번호
    
    ```sql
    -- ROWNUM 조회하기
    SELECT ROWNUM, ENAME 
    FROM EMP
    WHERE DEPTNO=10;
    ```
    
- WHERE 절의 조건을 만족한 후의 결과
- ROWNUM 조회 조건 만들 시 주의 할 점
    - 조건 중 ROWNUM >X는 불가 → WHERE 절에서 필터링 됨
    - ORDER BY ROWNUM은 의미 없음 → 이미 정렬 완료

## FETCH OFFSET 절

> 오라클에서 데이터 조회 시  행 개수 제한하는 방법
> 

- OFFSET은 시작 위치, FETCH는 행의 개수
- Oracle 12c 버전부터 지원

```sql
-- 급여 내림차순 정렬하여 5개 데이터 조회
SELECT *
FROM EMP
ORDER BY SAL DESC
OFFSET 0
ROWS FETCH NEXT 5 ROWS ONLY;
```

- SYNONYM, SEQUENCE
    
    ## Synonym
    
    > 기존 객체에 별명을 붙이는 것
    > 
    
    ### Synonym 사용하는 목적
    
    - 응용 프로그램과 테이터베이스 연결 시 유지보수가 편리해 짐
        - 동의어만 변경하면 됨
    
    ### Synonym 생성
    
    ```sql
    CREATE SYNONYM 이름 FOR 기존 객체명;
    ```
    
    ### Synonym 삭제
    
    ```sql
    DROP SYNONYM 이름;
    ```
    
    ## Sequence
    
    > 오라클의 일련번호, 여러개 사용 가능하며 키의 제한이 없음
    > 
    
    ### MySQL과 Oracle의 차이점
    
    - MySQL에서는 일련번호 생성 시 AUTO_INCREMENT 사용
    - MySQL에서는 하나의 테이블에 하나의 일련번호 사용해야 함
    - MySQL에서는 반드시 Primary key 또는 UNIQUE 제약조건 설정 필요
    
    ### Sequence 생성
    
    ```sql
    CREATE SEQUENCE 이름
    [START WITH 초기값]
    [MAXVALUE(최대값)|NOMAXVALUE]
    [MINVALUE(최소값)|NOMINVALUE]
    [CYCLE|NOCYCLE]
    [CACHE 개수|NOCACHE]
    ```
    
    ### Sequence 생성 및 값 확인
    
    ```sql
    -- SEQUENCE 생성
    CREATE SEQUENCE SEQDATA1
    	START WITH 1000
    	INCREMENT BY 10;
    
    -- SEQUENCE 값 확인
    SELECT SEQDATA1.NEXTVAL
    FROM DUAL;
    ```
    
    ### Sequence 사용
    
    - **이름.currval**
        - 현재 값
    - **이름.nextval**
        - 다음 값
    
    ### Sequence 삭제
    
    ```sql
    DROP SEQUENCE 이름
    ```
    
    ### Sequence 변경
    
    ```sql
    ALTER SEQUENCE 이름
    START WITH 제외 모든 옵션 수정 가능
    ```
    
    ### Sequence를 이용한 데이터 삽입
    
    ```sql
    -- 시퀀스 이용한 데이터 삽입
    INSERT INTO DEPT(DEPTNO,DNAME,LOC) VALUES(SEQDATA1.NEXTVAL,'개발','판교');
    ```
    
- ROLLUP 함수
    
    ## ROLLUP 함수
    
    > 그룹별로 중간 / 전체 평균 등을 조회 할 때 사용
    > 
    
    - NVL 이용하여 합산 결과 명 지정 가능
        
        ```sql
        -- EMP 테이블에서 JOB별로 SAL의 평균/합계를 조회
        SELECT NVL(JOB,'TOTAL'), AVG(SAL) 급여평균
        FROM EMP
        GROUP BY ROLLUP(JOB);
        ```
        
    - ROLLUP에 설정하는 컬럼은 숫자 데이터는 안됨
        - 숫자 컬럼의 경우는 DECODE 함수로 조회
            
            ```sql
            -- DEPTNO별로 SAL의 합계를 조회
            -- 숫자 컬럼은 조회 시 DECODE 함수 이용해야 함
            -- DECODE 값이 NULL이면 전체, 아니면 DEPTNO 변환하여 조회
            SELECT DECODE(DEPTNO,NULL,'TOTAL',DEPTNO), SUM(SAL) 급여합계
            FROM EMP
            GROUP BY ROLLUP(DEPTNO);
            ```

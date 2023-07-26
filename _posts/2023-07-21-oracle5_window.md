---
title: "Oracle의 Window 함수"
excerpt: "오라클의 윈도우 함수"

categories:
  - database
tags:
  - Oracle
last_modified_at: 2023-07-21
---

## WINDOW 함수

> 행과 행 간의 관계 정의하기 위해 사용하는 함수 (순위, 합계, 평균, 누적 합, 비율 등)
> 

### WINDOW 함수의 형식

```sql
SELECT WINDOW_FUNCTION(매개변수)
OVER(PARTIRION BY 그룹화 할 컬럼명 
ORDER BY WINDOWING)
FROM 테이블명; 
```

- **WINDOW_FUNCTION**
    - 윈도우 함수 옵션(순위, 함계, 평균, 누적 합, 비율 등)
- **매개변수**
    - 함수 따라 다르게 적용
- **PARTITION BY**
    - 생략 시 데이터 전체
- **WINDOWING**
    - 정렬 기준

### WINDOW 함수 실습

- EMP 테이블 전체 SAL에서 나의 SAL 비율 예제
    - SAL과 SUM(SAL)의  Cardinality가 달라서 아래 코드는 에러
        
        ```sql
        SELECT ENAME, SAL, SAL/SUM(SAL)
        FROM EMP;
        ```
        
    - WINDOW 함수 이용하여 위의 에러 해결
        
        ```sql
        -- SUM(SAL) 전부 복사하여 cardinality 맞춰줌
        SELECT ENAME, SAL, SAL*100/SUM(SAL) OVER() AS "급여 비율"
        FROM EMP;
        ```
        

### WINDOW의 범위 설정 - OVER( )

- 시작 및 종료 위치 범위 설정 가능  안에 작성
- ROWS BETWEEN  시작위치 AND 종료위치
- RANGE BETWEEN  시작위치 AND 종료위치
- 시작위치 지정 방법
    
    ```sql
    UNBOUNDED PRECEDING -- 처음부터 
    CURRENT ROW -- 현재 행부터
    N PRECEDING -- N번째 앞 행부터 
    ```
    
- 종료위치 지정 방법
    
    ```sql
    UNBOUNDED FOLLOWING -- 마지막까지
    CURRENT ROW -- 현재 행까지
    N FOLLOWING -- N번째 행까지
    ```
    
- 예제
    
    ```sql
    -- EMP 테이블에서 EMPNO, ENAME, SAL, 현재행까지 SAL 합계 조회
    SELECT EMPNO, ENAME, SAL, SUM(SAL) OVER(
    ORDER BY SAL ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) "TOTAL"
    FROM EMP;
    ```
    

### WINDOW  그룹별 조회

- OVER( ) 안에 PARTITION 이용
    
    ```sql
    -- 부서별 급여 평균 PARTITION 이용
    SELECT EMPNO, ENAME, SAL, ROUND(AVG(SAL) OVER(PARTITION BY DEPTNO)) "부서별 평균 급여"
    FROM EMP;
    ```
    

### WINDOW 순위 함수

- **ROW_NUMBER( )**
    - 동일한 순위 존재 않는 일련번호 형태 순위
- **DENSE_RANK( )**
    - 동일한 순위는 동일하게 출력하고 다음 순위를 연속해서 부여
- **RANK( )**
    - 동일할 경우 다음 건너뛰고 순위 부여
        
        ```sql
        -- 부서별 SAL 순위 부여
        SELECT ENAME, SAL, DEPTNO, RANK() OVER(PARTITION BY DEPTNO ORDER BY SAL DESC) "급여 순위"
        FROM EMP;
        ```
        

### 행 순서 관련 함수

- **FIRST_VALUE(컬럼명)**
    - 그룹의 첫번째 행
- **LAST_VALUE(컬럼명)**
    - 그룹의 마지막 행
- **LAG(컬럼명)**
    - 이전 행
- **LEAD(컬럼명, 인덱스)**
    - 인덱스 만큼 건너 뛴 행

### 비율 관련 함수

- **CUME_DIST( )**
    - 누적 분포 상의 비율 0-1 사이의 값
- **PERCENT_RANK( )**
    - 0부터 1의 누적 비율
- **NTILE(분할 개수)**
    - 분할 한 후 위치를 조회
- **RATIO_TO_REPORT( )**
    - 합계에 대한 컬럼 값의 백분율

### PIVOT

- PIVOT(집계함수)
- 예시
    
    ```sql
    -- PIVOT  예시
    SELECT * FROM EMP
    PIVOT(MAX(SAL),FOR DEPTNO IN (10,20,300;)
    ```
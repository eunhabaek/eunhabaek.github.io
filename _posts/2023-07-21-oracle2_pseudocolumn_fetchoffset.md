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

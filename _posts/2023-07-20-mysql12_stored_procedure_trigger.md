---
title: "MySQL의 Stored Procedure와 Trigger "
excerpt: "MySQL의 저장 프로시저와 트리거"

categories:
  - database
tags:
  - MySQL
last_modified_at: 2023-07-20
---

## Stored Procedure란?

> 자주 사용하는 구문을 함수처럼 하나로 묶어두고 이름만으로 사용하는 것 = 절차적 프로그래밍 (ORACLE의 PL/SQL)
> 

- 함수와 다른 점
    - stored procedure는 리턴하지 않을 수 있음
- 목적
    - **속도**: 한번 호출되면 메모리에 적재되어 수행하기 때문에 실행 속도가 빠름
    - **보안**: 외부에서는 내부 구조를 알지 못하고 작업 수행 가능
- 단점
    - 데이터베이스 종류마다 생성 방식이 다름
- 포트폴리오 만들 때 ORM 사용하지 않는다면 프로시저를 사용하는 것이 좋음
- 생성

    ```sql
    DELIMITER 기호기호
    	CREATE [OR REPLACE] 프로시저명()
    	BEGIN 
    			SQL 작성
    	END 기호기호
    DELIMITER ;
    ```

- 생성 예시
    
    ```sql
    -- DELIMITER는 프로시저 종료를 알리기 위한 기호 설정
    -- DBeaver에서 수행할때는 SQL 스크립트 실행으로 
    DELIMITER //
    CREATE PROCEDURE PROC1(USID CHAR(15),
                            USERNAME VARCHAR(20),
                            BDAY INT(11),
                            ADDR CHAR(100),
                            PHONE CHAR(11),
                            DT DATE)
            BEGIN
                INSERT INTO usertbl
                VALUES(USID,USERNAME, BDAY, ADDR, PHONE,DT);
            END //
    DELIMITER ;
    ```
        
- 호출
    - CALL 프로시저명(매개변수 나열)
        
        ```sql
        CALL PROC1('EUNHA','은하',1016,'서울','01012345678','2023-07-20');
        ```



## Trigger란?

> DML 작업 발생 전 후에 특정 작업을 자동으로 수행하는 것
> 

💡 트리거를 통해 유효성 검사, 로깅 등 common concern을 수행  

- 형식
    
    ```sql
    DELIMITER //
    CREATE TRIGGER 트리거명 
    [BEFORE|AFTER] [INSERT|UPDATE|DELETE]
    ON 테이블명
    [FOR EACH ROW] -- 행 개수 만큼 수행할 지 여부
    [WHEN 조건]
    BEGIN
    	수행할 내용
    END //
    DELIMITER ;
    ```
    
- 권한 문제로 트리거 생성이 안될 경우
    - 관리자 계정으로 접속해서 명령 수행
    - 관리자 계정에서 set global log_bin_trust_function_creators=on; 명령어 실행
- 예시
    
    ```sql
    -- trigger 수행 위한 테이블 생성
    CREATE TABLE EMP1(
    	EMPNO INT PRIMARY KEY,
    	ENAME VARCHAR(30) NOT NULL,
    	JOB VARCHAR(100)
    );
    
    CREATE TABLE SAL1(
    	SALNO INT PRIMARY KEY AUTO_INCREMENT,
    	SAL FLOAT(7,2),
    	EMPNO INT REFERENCES EMP1(EMPNO) ON DELETE CASCADE
    );
    
    -- EMP1 테이블에 데이터 추가 할 시 SAL1에도 데이터 자동으로 추가되는 트리거 생성
    DELIMITER //
    CREATE TRIGGER EMP_SAL_ADD_TRIG
    AFTER INSERT ON EMP1
    FOR EACH ROW
    BEGIN
    	INSERT INTO SAL1(SAL,EMPNO) VALUES(100,NEW.EMPNO);
    END //
    DELIMITER ;
    
    -- EMP1에 INSERT 실행 시 트리거로 인해 SAL1에도 데이터 삽입
    INSERT INTO EMP1 VALUES(1,'EUNHA','STUDENT')
    ```
    
- 내용 안에서 삽입 또는 수정되는 값을 사용할때는 NEW.컬럼명
- 수행할 내용 안에서 수정되기 전 값이나 삭제되는 값을 사용할 때는 OLD.컬럼명 이용
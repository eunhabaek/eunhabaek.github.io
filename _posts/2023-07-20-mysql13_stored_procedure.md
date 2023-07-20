---
title: "MySQL Stored Procedure"
excerpt: "MySQL의 저장 프로시저"

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
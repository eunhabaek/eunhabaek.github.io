---
title: "MySQL DDL (2) "
excerpt: "MySQL의 테이블 생성 시 기본값 설정과 제약 조건"

categories:
  - database
tags:
  - MySQL
last_modified_at: 2023-07-19
---

## DEFAULT

- 지정 값 없을 시 기본 값
- 형식: 컬럼명 자료형 DEFAULT  “기본값”
    
    ```sql
    -- DEFAULT 설정
    CREATE TABLE NOTNULLTB(
    	NAME CHAR(10) NOT NULL,
    	AGE INTEGER DEFAULT 0 -- 값 입력 없을 시 0으로 저장
    );
    ```
    
- 기본값 사용하기 위해서는 컬럼 제외 입력하거나 DEFAULT로 입력

## CONSTRAINT(제약조건)

### 1. Data integrity (무결성)

- 데이터의 정확성과 일관성 유지/보증
- **개체 무결성**(Entity integrity): 기본키는 NULL이거나 중복 불가
- **참조 무결성**(Reference integrity): 외래키, 참조키는 NULL이거나 참조 가능 값만 가능
- **도메인 무결성**(Domain integrity): 각 컬럼에는 설정한 도메인 값만 저장

### 2. NOT NULL

- 필수 입력 위한 제약조건
- **컬럼에만 설정 가능**
- 데이터베이스에서는 NULL을 저장하지 않고 별도 공간을 할당하여 확인→ NOT NULL은 크기가 1 큼

### 3. CHECK

- 속성의 값을 제한하는 제약 조건
- 형식: CHECK(컬럼명 조건)
    
    ```sql
    -- NAME, GENDER(남 또는 여), AGE(0-100)를 속성으로 갖는 테이블 생성
    CREATE TABLE INFO(
    	NAME CHAR(10) NOT NULL,
    	GENDER CHAR(3) CHECK(GENDER IN ('남','여')),
    	SCORE INTEGER CHECK(SCORE BETWEEN 0 AND 100)
    );	
    -- 정상 입력
    INSERT INTO INFO(NAME,GENDER,SCORE) VALUES('EUNHA','여','27');
    -- GENDER 컬럼 범위 에러
    INSERT INTO INFO(NAME,GENDER,SCORE) VALUES('EUNHA','김','27');
    -- SCORE 컬럼 범위 에러
    INSERT INTO INFO(NAME,GENDER,SCORE) VALUES('EUNHA','여','-27');
    ```
    

### 4. Primary key

- 기본키는 테이블 당 하나만 설정 가능 → 두 번 쓰면 에러
- 컬럼 및 테이블 제약 조건으로 설정 가능
    - **컬럼 제약 조건**: 자료형 뒤에 설정
        - 컬럼명 자료형 [CONSTRAINT 제약조건명] PRIMARY KEY
            
            ```sql
            -- 기본 키 컬럼제약조건 설정 및 이름 생성
            CREATE TABLE PKTABLE1(
            	NAME CHAR(10) CONSTRAINT PKTABLE1_PK PRIMARY KEY,
            	AGE INT
            );
            ```
            
    - **테이블 제약 조건**: 컬럼 정의 후 제약 조건 설정
        - [CONSTRAINT 제약조건명] PRIMARY KEY(컬럼명 나열)
            
            ```sql
            -- 기본 키 테이블제약조건 설정 및 이름 생성
            CREATE TABLE PKTABLE2(
            	NAME CHAR(10), 
            	AGE INT,
            	CONSTRAINT PKTABLE2_PK PRIMARY KEY(NAME)
            );
            ```
            
- 여러개의 속성으로도 설정 가능
    - 여러개의 속성으로 설정 시 테이블 제약조건으로 설정 필요 → 여러번 사용 불가
- 기본 키 설정시 인덱스 생성되고 MySQL은 기본키 순서대로 저장
- 제약조건 이름을 만드는 이유 - 관리 편리하게 하기 위함
- 기본키는 **NOT NULL**이고 **UNIQUE**
    - 같은 값 중복 입력 하거나 입력하지 않을 시 에러
    

### 5. UNIQUE

- 중복 값 허용하지 않는 제약조건
- NULL 가능
    - NULL은 중복 가능
- 설정법은 Primary key와 동일
- 인덱스 자동 생성되나, 순서대로 저장되지않음
    
    ```sql
    -- EMPLOYEETABLE 테이블 생성
    CREATE TABLE EMPLOYEETABLE(
    	NAME CHAR(10) UNIQUE, 
    	SAL INT NOT NULL,
    	ADDR VARCHAR(30) NOT NULL,
    	CONSTRAINT EMPLOYEETABLE PRIMARY KEY(NAME)
    );
    
    -- 데이터 삽입
    INSERT INTO EMPLOYEETABLE(NAME, SAL,ADDR) VALUES('EUNHA',20,'광주');
    INSERT INTO EMPLOYEETABLE(NAME, SAL,ADDR) VALUES('CRYSTAL',40,'대전');
    INSERT INTO EMPLOYEETABLE(NAME, SAL,ADDR) VALUES('ZEA',60,'서울');
    ```
    

### 6. FOREIGN KEY(외래키)

- 다른 테이블을 참조하기 위한 속성
- 다른 테이블에서는 기본키이거나 UNIQUE한 속성이어야 함
- 설정 방법
    - **컬럼제약조건**
        - REFERENCES 참조할테이블 이름(컬럼이름)[옵션]
        - 버전에 따라 제공 안하기도 하므로 지양
            
            ```sql
            -- 컬럼에서 외래키 설정하기
            CREATE TABLE PROJ(
            	PROJECTID INT PRIMARY KEY,
            	EMPLOYEE CHAR(10) NOT NULL REFERENCES EMPLOYEETABLE(NAME), -- 외래키 참조
            	PROJECT VARCHAR(30) NOT NULL,
            	COST INT
            );
            ```
            
    - **테이블제약조건**
        - [CONSTRAINT 이름] FOREIGN KEY(외래키 컬럼명) REFERENCES 참조할테이블 이름(컬럼이름)
            
            ```sql
            -- 테이블에서 외래키 설정하기
            CREATE TABLE PROJ(
            	PROJECTID INT PRIMARY KEY,
            	EMPLOYEE CHAR(10) NOT NULL,
            	PROJECT VARCHAR(30) NOT NULL,
            	COST INT,
            	 -- 외래키 참조
            	CONSTRAINT FK_EMPLOYEETABLE FOREIGN KEY(EMPLOYEE) REFERENCES EMPLOYEETABLE(NAME)
            );
            ```
            
- 옵션 없이 외래키 설정 시 외래키의 값은 NULL이거나 참조할 테이블에 존재하는 값만 삽입 가능
    
    ```sql
    -- EUNHA는 EMPLOYEETABLE에 있어서 삽입 가능
    INSERT INTO PROJ(PROJECTID, EMPLOYEE, PROJECT, COST) VALUES(1,'EUNHA','웹서비스',4000);
    -- HANA는 EMPLOYEETABLE에 없어서 삽입 불가
    INSERT INTO PROJ(PROJECTID, EMPLOYEE, PROJECT, COST) VALUES(2,'HANA','클라우드',3000);
    ```
    
- 참조당하는 테이블 삭제 불가
    
    ```sql
    -- EMPLOYEETABLE 테이블은 PROJ 테이블에서 참조 중이므로 삭제 불가능
    DROP TABLE EMPLOYEETABLE;
    ```
    
- 참조당하는 테이블에서는 참조되는 데이터 삭제 불가
    
    ```sql
    -- EUNHA는 PROJ 테이블에서 참조 중이므로 삭제 불가능
    DELETE FROM EMPLOYEETABLE WHERE NAME='EUNHA'; -- -> 에러
    ```
    

### 7. 외래키 제약 조건의 옵션

- 형식
    - **ON DELETE**\{ NO ACTION | CASCADE | SET NULL | SET DEFAULT \} 4가지 중 선택
    - **ON UPDATE**\{ NO ACTION | CASCADE | SET NULL | SET DEFAULT \} 4가지 중 선택
- **NO ACTION**
    - 참조하는 테이블에 변화 생겨도 아무 액션 취하지 않음
- **CASCADE**
    - 참조하는 테이블의 변화에 따라 데이터가 같이 삭제되거나 같이 변경됨
        
        ```sql
        CREATE TABLE PROJ(
        	PROJECTID INT PRIMARY KEY,
        	EMPLOYEE CHAR(10) NOT NULL,
        	PROJECT VARCHAR(30) NOT NULL,
        	COST INT,
        	 -- 외래키 참조
        	CONSTRAINT FK_EMPLOYEETABLE FOREIGN KEY(EMPLOYEE) REFERENCES EMPLOYEETABLE(NAME)
        	ON DELETE CASCADE ON UPDATE CASCADE -- 같이 수정되고 같이 삭제되는 옵션 설정
        );
        
        -- EMPLOYEETABLE에서 EUNHA이름 변경 시 PROJ 테이블도 같이 변경
        UPDATE EMPLOYEETABLE SET NAME='BAEKEUNHA' WHERE NAME='EUNHA';
        
        -- EMPLOYEETABLE에서 BAEKEUNHA 삭제 시 PROJ 테이블에서도 같이 삭제 됨
        DELETE FROM EMPLOYEETABLE WHERE NAME='BAEKEUNHA';
        ```
        
- **SET NULL**
    - 참조하는 테이블의 변화에 따라 참조되는 데이터 NULL로 변경
- **SET DEFAULT**
    - 참조하는 테이블의 변화에 따라 참조되는 데이터 기본 값으로 변경

### 8. AUTO_INCREMENT

- MySQL에서 일련번호 설정 위해 사용
- 하나의 테이블에 한번만 사용 가능
- AUTO_INCREMENT 설정된 컬럼은 UNIQUE나 PRIMARY KEY 제약 조건 설정이 필수
    
    ```sql
    -- 일련번호 사용하여 속성 만들기
    CREATE TABLE NUMERS(
    	NUM INT AUTO_INCREMENT PRIMARY KEY, 
    	TITLE CHAR(100),
    	CONTENT TEXT
    );
    
    INSERT INTO NUMERS(TITLE,CONTENT) VALUES('제목1','내용1'); -- NUM은 1로 자동 생성
    INSERT INTO NUMERS(TITLE,CONTENT) VALUES('제목2','내용2'); -- NUM은 2로 자동 생성
    ```
    
- 중간 행 삭제해도 숫자 변하지 않음
- AUTO_INCREMENT가 설정되어 있어도 직접 값 입력 가능 - 지양
    
    ```sql
    INSERT INTO NUMERS(NUM,TITLE,CONTENT) VALUES(10,'제목3','내용3'); -- NUM 직접 생성 가능
    INSERT INTO NUMERS(TITLE,CONTENT) VALUES('제목4','내용4'); -- NUM은 11로 자동 생성됨
    ```
    
- 테이블 만들 때 초기 값 설정 가능하며, ALTER TABLE 명령어로 초기 값 변경 가능
    
    ```sql
    -- 초기 값 변경 가능 
    ALTER TABLE NUMERS AUTO_INCREMENT=1000;
    INSERT INTO NUMERS(TITLE,CONTENT) VALUES('제목5','내용5'); -- NUM은 1000으로 자동 생성됨
    ```
    

### 9. 제약조건 작업

- **제약조건 조회**
    - FROM 절에 information_schema.table_constraints 작성
        
        ```sql
        -- 모든 제약조건 조회
        SELECT *
        FROM information_schema.table_constraints;
        ```
        
- **제약조건 수정**
    - 형식:
        
        ALTER TABLE 테이블명  
        
        MODIFY 컬럼명 자료형 제약조건;  
        
- **제약조건 추가**
    - 형식:
        
        ALTER TABLE 테이블명  
        
        ADD 테이블 제약조건;  
        
- **제약조건 삭제**
    - 형식:
        
        ALTER TABLE 테이블명  
        
        DROP 제약조건명;
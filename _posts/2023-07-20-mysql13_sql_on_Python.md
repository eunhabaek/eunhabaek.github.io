---
title: "MySQL과 python 연동하기"
excerpt: "pyMySQL을 이용하여 파이썬에서 MySQL 작업하기"

categories:
  - database
tags:
  - MySQL
last_modified_at: 2023-07-20
---
## python과 MySQL 연동

### pyMySQL 드라이버 이용 연결하기

> SQL 직접 작성하는 방법
> 

1. 파이썬과 MySQL 연결을 위한 드라이버 패키지 다운로드 및 설치
    
    ```bash
    pip install pyMySQL
    ```
    
2. 데이터베이스와 연결
    - 연결을 위한 옵션
        - **host**: IP 혹은 localhost
        - **port**: MySQL은 3306
        - **db**: MySQL은 DB명, Oracle은 서비스 명이나 SID
        - **user**: 사용자명
        - **passwd**: 암호
        - **charset**: 인코딩 방식
3. 데이터베이스 연결
    - 형식
        
        ```python
        import pymysql
        변수=pymysql.connect(host=,port=,user=,passwd=,db=,charset=)
        ```
        
    - 성공하면 메세지 없고 실패하면 에러 메세지와 함께 예외 발생
    - 연결 종료
        
        ```python
        변수.close()
        ```
        
    
4. 연동 예시
    
    ```python
    import pymysql
    import sys
    
    try:
        connect=pymysql.connect(host='localhost',port=3306,db='mysql',
                                user='root',passwd='beh1016',charset='utf8')
        print(connect)
    except:
        print("pymysql 예외발생",sys.exc_info())
    finally:
        connect.close()
    ```
    

### DML 작업하기

- 로직: 데이터베이스 연결 객체가 cursor 함수를 호출하여 SQL 실행 객체를 리턴
- SQL 실행 객체.execute(SQL 문장[,파라미터값 나열])
    - 파라미터 값 직접 삽입  및 서식 설정 대입 가능
    - 보안 상 서식 설정 대입 지향
- 데이터베이스 연결 객체.commit( ) 호출하면 원본에 반영되고, rollback( ) 호출 시 원본에 반영 X
- 데이터 삽입
    1. 작업할 테이블 확인
        
        DBeaver등 SQL 접속 도구에서 수행  
        
    2. 파이썬에서 DML 실행
        
        ```python
        import pymysql
        import sys
        try:
            #연결 객체 생성
            connect=pymysql.connect(host='localhost',port=3306,db='mysql',
                                    user='root',passwd='beh1016',charset='utf8')
            #SQL 실행 객체 생성
            cursor=connect.cursor()
        
            cursor.execute("USE dx0717")
            # SQL 실행 (1) - 값 직접 sql에 작성
            cursor.execute("INSERT INTO DEPT VALUES(11,'인사관리','서울')")
        
            # SQL 실행 (2) - 서식 설정하여 대입
            cursor.execute("UPDATE DEPT SET DNAME=%s LOC=%s WHERE DEPNO=%s",('개발','부산',11))
        
            #원본에 반영
            connect.commit()
        
            print(connect)
        except:
            print("pyMySQL 예외 발생!!",sys.exc_info())
        finally:
            if connect !=None:
                connect.close()
        ```
        

### 3. DQL 작업하기

- cursor 이용하여 select 구문 수행
- fetch 메서드 → 원본 보호 위해서 튜플로 리턴
    - **fetchone** 메서드 호출
        - 하나의 데이터만 튜플로 리턴
        - 검색 결과 없는 경우 빈 튜플 리턴
            - ⚠️None이 아님
            - 반복문 고려하여 None을 리턴하지 않음
    - **fetchall** 메서드 호출
        - 튜플의 튜플로 모든 데이터 리턴
        - 검색 결과 없을 시 None 출력
- 예시
    
    ```python
    import pymysql
    import sys
    try:
        #연결 객체 생성
        connect=pymysql.connect(host='localhost',port=3306,db='mysql',
                                user='root',passwd='beh1016',charset='utf8')
        #SQL 실행 객체 생성
        cursor=connect.cursor()
    
    		#use 테이블 선언
        cursor.execute("USE dx0717;")
    
        # SQL DQL 데이터 조회
        cursor.execute("SELECT * FROM DEPT WHERE DEPTNO >5;")
    		
    		# 검색 결과 전체 읽기
        record=cursor.fetchall()
    
        if len(record)==0:
            print('검색된 데이터 없음')
        else:
            for elem in record:
                print(elem)
    
        #원본에 반영
        connect.commit()
    
        print(connect)
    except:
        print("pyMySQL 예외 발생!!",sys.exc_info())
    finally:
        if connect !=None:
            connect.close()
    ```

### 드라이버, 프레임워크 이용

- SQL Mapper
    - 소스 코드와 SQL을 분리시켜서 수행
    - MyBatis가 대표적 프레임워크→ 우리나라 SI업계, 쉽지만 효율 낮음
- ORM(Object relation model)
    - 프로그래밍 언어의 인스턴스와 관계형 데이터베이스의 행을 매핑시켜서 SQL 사용 없이 데이터베이스 작업 할 수 있도록 해주는 방식
    - 데이터베이스 변경해도 수정이 거의 발생하지 않음
    - 효율은 좋으나 어려워서 솔루션 업체에서 이용
    - 최근 많이 사용
    - Django 또는 JAVA의 JPA 등이 대표적 ORM 프레임워크
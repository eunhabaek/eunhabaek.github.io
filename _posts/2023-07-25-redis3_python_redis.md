---
title: "Redis와 파이썬 연동"
excerpt: "Redis with python"

categories:
  - database
tags:
  - Redis
last_modified_at: 2023-07-25
---
## Connection Pool

- 데이터베이스 프로그래밍을 할 때 직접 연결을 생성해서 사용 후 반납(해제)하는 방식
- 미리 연결을 만들어두고 빌려쓰는 방식 이용
- 오버헤드를 줄일 수 있음

## Python에서 Redis 연동

### redis 모듈 import

- redis 모듈 없을 경우 터미널에서 pip install redis 실행
    
    ```python
    import redis
    ```
    

### 데이터베이스 연결 객체 생성

- 전통 방식
    - try:
        
        외부 자원에 연결  
        
        except:  
        
        예외가 발생했을 때 처리  
        
        finally:  
        
        외부 자원 연결 해제  
        
- with  구문 이용한 방식
    - with 외부자원에 연결 as 이름:
        
        ```python
        with redis.StrictRedis(host='localhost', port=6379) as connect:
        ```
        
        - with 안에서 외부 자원에 연결하면 예외 발생 여부에 상관없이 블럭을 나갈 때 연결 해제

### 문자열 저장하기

- 연결객체.메소드( 키, 값) 형식으로 이용
    
    ```python
    connect.set("name", "eunha")
    ```
    

### 문자열 가져오기

- 기본 출력은 bytes로 리턴
    
    ```python
    data = connnect.get("name")
    print(data) #인코딩 결과가 출력됨
    print(data.decode('utf-8')) #인코딩 결과가 출력됨
    ```
    

### Connection Pool 이용 접속

- 만료시간 지정하여 접속하기
    
    ```python
    import time
    
    #connection pool 객체 생성
    redis_pool=redis.ConnectionPool(host='localhost', port=6379,max_connections=4)
    
    #redis 연결 객체 생성
    with redis.StrictRedis(connection_pool=redis_pool) as conn:
        # 만료 시간 설정 가능
        conn.set("name", "eunha", 10)  # 만료시간 10초
    
        # 만료 시간 확인
        print("만료시간: ",conn.ttl("name"))
        
        #데이터 값 확인
        print("name: ",conn.get("name"))
        
        #11초 sleep
        time.sleep(11)
        
        #11초 지난 후 데이터 값 확인-> 만료되어서 None
        print("11초 후 name: ",conn.get("name"))
    ```
    

### Redis 리스트에 데이터 저장

- 리스트 생성 후 for문으로 print 해보기
    
    ```python
    conn.lpush("animals", "cat", "dog", "lion")
    conn.rpush("animals", "rabbit","monkey")
    
    for i in conn.lrange("animals", 0, 5):
    		print(i)
    ```
    
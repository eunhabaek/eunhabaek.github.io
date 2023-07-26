---
title: "MongoDB의 데이터 조회"
excerpt: "MongoDB에서 외부 파일을 import 하고 데이터를 조회하기"

categories:
  - database
tags:
  - MongoDB
last_modified_at: 2023-07-23
---

## 외부 JSON로 데이터 생성

- 도커 컨테이너에 파일 복사 명령어 : 환경설정할 시에 이용
- 터미널에서 작업
- 컨테이너의 tmp 디렉토리에  json 파일 복사하기
    
    ```bash
    #docker cp 파일경로 컨테이너명:/복사디렉토리/파일명
    docker cp C:\Users\USER\Downloads\area.json mongodb_eunha:/tmp/area.json
    ```
    

## 외부 JSON 파일 읽어오기

- 접속 후 bash shell에서 수행
    
    ```bash
    #mongoimport -d 데이터베이스명 -c 컬랙션명 < 파일경로
    mongoimport -d table1 -c area < /tmp/area.json
    ```
    

## 불러온 데이터 확인

- mongo shell에서 수행 (mongosh)
    
    ```bash
    use 데이터베이스
    db.컬랙션명.find()
    ```
    

## MongoDB 데이터 조회

- 단일 도큐먼트에서만 조회
- join이 불가능함
    - 정규화 → DQL 느리나 DML 빠름, join 多
    - 역정규화 → DQL 빠르나 DML 느림
- 여러개 객체 사이 관계 만들 시 linking이나 embedding (배열 객체 안에 파일 등 배열 객체 넣기 )이용
    - **embedding** ⇒ 안정화는 떨어지나, 조회가 빠름
    

## MongoDB의 데이터 조회- find( )

- collection의 내용 확인
    
    ```jsx
    */전체 데이터 조회*/
    db.컬랙션명.find()
    ```
    
- 매개변수 2개(쿼리, 조회할 필드)
- 리턴은 cursor

### 1. 조건 검색

- 일치하는 데이터 조회
- 조건 값 여러개 입력 시 and 조건으로 간주
    
    ```jsx
    /*db.컬랙션명.find({속성값1:값1,속성값2:값2...})*/
    db.coll1.find({name:"eunha"})
    ```
    

### 2. 특정 속성(컬럼)만 조회
- 추출할 컬럼에만 TRUE(1)
    ```jsx
    db.컬랙션명.find({조건},{컬럼명:true(1) or false(0),...})

    db.coll1.find({name:"eunha",age:27},{age:1}) /*age만 조회*/
    ```

### 3. 비교 연산자 이용 조회

- 사용방법
    
    ```jsx
    /*{컬럼명:{연산자1:값1,연산자2:값2,...}}*/
    ```
    
- 종류
    - **$eq**: 같다
        - coll1 컬랙션에서 name의 값이 eunha인 경우 조회
            
            ```jsx
            db.coll1.find({name:{$eq:"eunha"}})
            ```
            
    - **$ne**: 같지않
    - **$gt**: 크다
    - **$gte**: 크지 않다
    - **$lt**: 작다
    - **$lte**: 작지 않다
    - **$in**: 배열의 요소 중 하나
        - coll1 컬랙션에서 name의 값이 eunha나 crystal인 경우 조회
            
            ```jsx
            db.coll1.find({name:{$in:["eunha","crystal"]}}
            ```
            
    - **$nin**: 배열의 요소가 아닌
- 배열에 정규식 사용 가능
    - name이 영문 소문자로 시작하는 요소 찾기
        
        ```jsx
        db.coll1.find({name:{$in:[/^[a-z]/]}})
        ```
        

### 4. 논리 결합 연산자 이용 조회

- **$not**
    - age가 25보다 크지 않은 데이터 조회
        
        ```jsx
        db.coll1.find({age:{$not:{$gt:25}}})
        ```
        
- **$and**
- **$or**
    - age가 25보다 크거나 30보다 작은 데이터 조회
        
        ```jsx
        db.coll1.find({$or:[{age:{$gt:25}},{age:{$lt:30}}]})
        ```
        
- **$nor**: 주어진 조건 중 하나도 만족하지 않는 데이터 조회
- $or, $in, $nin 사용하지 않으면 순서를  고려해야 함
    
    순서 동일하게 tags에 red, blank 포함하는 데이터 조회
    
    ```jsx
    db.inventory.find({tags:["red","blank"]})
    /*["blank","red"]와는 다른 결과를 조회*/
    ```
    

### 5. 문자열 조회

- **$regex**: 정규표현식
    - name에 s 포함하는 데이터 조회
        
        ```jsx
        db.coll1.find({name:/s/})
        ```
        
    - name이 e로 시작하는 데이터 조회
        
        ```jsx
        db.coll1.find({name:/^e/})
        ```
        
    - name이 a로 끝나는 데이터 조회
        
        ```jsx
        db.coll1.find({name:/e$/})
        ```
        
- **$text**: 문자열 검색

### 6. 배열 연산자 이용 조회

- RDBMS와 다른 점
- NoSQL은 객체 안에 배열 저장 가능
- 객체 안에 존재하는 배열을 이용하여 조회 가능함
- 종류
    - **$all**: **순서 상관없이** 배열안의 모든 요소가 포함되면 조회
        - tags에 순서 상관없이 red와 blank 가지는 데이터 조회
            
            ```jsx
            db.inventory.find({tags:{$all:["red","blank"]}})
            /*순서 상관 없이 결과를 조회*/
            ```
            
        
    - **$elemMatch:** 조건과 맞는 배열 속 요소를 선택
        - score의 하나라도 80보다 크고, 90보다 작은 데이터 조회
            
            ```jsx
            db.users.find({scores:{$elemMatch:{$gt:80, $lt:90}}})
            ```
            
    - **$size:** 배열의 크기가 같은 Document  선택
        - tags의 개수가 0인 데이터 조회
            
            ```jsx
            db.inventory.find({tags:{$size:0}})
            ```
            

### 7. 특정 인덱스의 데이터 조회 {$slice}

- {$slice:인덱스} : 인덱스번째까지 데이터 조회
    - 인덱스에 음수 가능
- {$slice:[start, end]} : 인덱싱 된 일부 데이터 조회 가능
    - 0번째부터 2번째까지의 tags 데이터만 조회
        
        ```jsx
        db.inventory.find({},{tags:{$slice:[0,2]}})
        ```
        

### 8. 컬럼 존재 유무확인 {$exists} → RDBMS에는 X

- $exists 연산자 이용하여 컬럼 존재여부 확인 가능
- NoSQL은 한 컬렉션 내의 데이터 모양이 다를 수 있기 때문에 확인이 필요함

### Cursor

> MongoDB에서 조회 성능을 높이기 위해 find 함수의 결과로 cursor를 리턴하는데, cursor란 query 결과를 가리키는 **포인터**(위치)이다. → DQL은 NoSQL이 효율적
> 

- RDBMS와의 차이점
    - RDBMS는 쿼리 결과로 실제 데이터를 리턴
    - 일부 데이터만 조회할 경우 cursor에 비해 메모리 비효율적
- cursor의 메서드
    - **hasNext( )**: 데이터 존재여부 리턴
    - **next( )**: 데이터 존재하는 경우 다음 데이터를 리턴

        ```jsx
        /*리턴된 cursor를 변수에 저장*/
        var cursor=db.inventory.find()
        
        /*cursor 이용하여 데이터 조회*/
        cursor.hasNext()
        cursor.next()
        
        /*삼항연산자로 위의 명령어 구현*/
        cursor.hasNext() ? cursor.next() : null
        ```
        
- 프로그래밍에서는 iterator, enumerator와 유사하며, 이를 쉽게 사용할 수 있도록 만든 제어문이 python의 for


## 기타 조회 함수

- **limit:**
    - 데이터 개수 제한 함수
    - skip과 함께 사용 가능
        
        ```jsx
        db.컬렉션명.find().skip(2).limit(6)
        ```
        
- **skip**
    - offset 변경함수
- **findOne**
    - 데이터 1개 조회 함수
    - unique한 속성을 가지고 데이터 조회
- **sort**
    - 정렬함수
    - 컬럼 이름과 정렬옵션 설정 : 1(오름차순), -1(내림차순)
        - id를 기준으로 오름차순하여 데이터 조회
            
            ```jsx
            db.inventory.find().sort(id:1)  
            ```

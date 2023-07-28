---
title: "Redis의 데이터 자료형"
excerpt: "Redis data type"

categories:
  - database
tags:
  - Redis
last_modified_at: 2023-07-25
---



## Redis의 문자열

- 저장
    - set 키 값
        
        ```bash
        set key1 value1
        ```
        
- 읽기
    - get 키
        
        ```bash
        get key1
        ```
        
- 삭제
    - del 키
- 전체 키 조회
    - keys *
- 여러 개의 데이터를 저장하거나 읽을 때는 mset 과 mget을 이용
- 여러 개의 데이터 조회
    - scan 페이지번호
    - 10개씩 조회하는데 첫번째 결과는 다음 데이터 존재 여부로 0이면 다음 데이터가 없음

## Redis의 리스트

- Linked List 로 Deque 처럼 동작
- 동일한 자료형의 데이터를 순차적으로 여러 개 모아놓은 구조
- lpush
    - 왼쪽에 삽입
    - lpush 키이름 데이터 나열
        
        ```bash
        lpush list1 a b c d e
        ```
        
- lrange
    - 왼쪽에서 데이터 조회
    - lrange 키이름 시작인덱스(0부터 시작) 종료인덱스(-1 이면 마지막 인덱스)
        
        ```bash
        lrange list1 0 -1
        ```
        
- rpush
    - 오른쪽에 삽입
- lpop, rpop
    - 왼쪽이나 오른쪽에서 하나의 데이터를 가져오고 리스트에서는 삭제
- llen 키이름
    - 데이터 개수
- ltim 키이름 시작인덱스 종료인덱스
    - 일정 범위의 데이터만 남기고 삭제
    - 없는 인덱스를 사용하면 모든 데이터를 삭제

## Redis의 set

- 중복된 데이터를 저장하지 않으며, 순서가 없는 자료구조
- 데이터를 member라고 부르는데 일반적으로 member의 존재 여부를 빠르게 확인하고자 할 때 사용합니다.
- redis 는 MSA 환경 같은 곳에서 많이 사용하기 때문에 공유 자원의 사용 여부를 set으로 저장하고 있다가 누가 사용 중인지 빠르게 확인하는 용도로 많이 사용합니다.
- 데이터 삽입
    - sadd 키이름 데이터나열
        
        ```bash
        sadd set1 abccc 
        #a,b,c 세개만 저장됨
        ```
        
- 모든 데이터 조회
    - smembers 키이름
- 데이터 존재 여부 확인
    - sismember 키이름 데이터
- 데이터 삭제
    - srem 키이름 데이터
- spop
    - 무작위로 데이터를 삭제하고 가져옴
- srandmember
    - 삭제하지 않고 데이터 가져옴

## Redis의 Sorted Set

- 정렬된 set
- score 와 value로 구성된 set
- score 로 정렬되고 score 가 같으면 value 로 정렬
- 실시간 순위표를 구현하고자 할 때 유용
- 데이터 삽입
    - zadd 키 score value score value ...
        
        ```bash
        zadd sset1 1 val1 2 val2 3 val3
        ```
        
- 인덱스를 이용한 조회
    - zrange 키 시작인덱스 종료인덱스 rev withscores limit 옵셋 개수
        
        ```bash
        zrange sset1 0 -1
        # 오름차순 출력
        ```
        
    - rev 옵션을 추가하면 역순으로 나오고 withscores를 추가하면 score도 같이 출력
        
        ```bash
        zrange sset1 0 -1
        # 내림차순 출력
        ```
        
- 값의 범위를 가지고 조회
    - zrangebyscore 키이름 최소값 최대값 withscores limit 옵셋 개수
        
        ```bash
        zrangebyscore sset1 1 2
        ```
        
    - zrevrangebyscore 키이름 최대값 최소값 withscores limit 옵셋 개수

## Redis의 Hash

- HashTable, Dictionary 와 유사한 개념
- key 와 value 를 같이 저장하는 자료구조
- key는 set으로 구성(중복할 수 없음)
- 저장
    - hset 과 hmset
        
        ```bash
        hset hash1 f1 val1 f2 val2 f3 val3
        ```
        
- 읽기
    - hget 과 hmget
        
        ```python
        hget hash1 f1
        ```
        
- hgetall
    - 모든 데이터 조회
- hkeys, hvals
    - 모든 key 와 value 조회
- hdel
    - 삭제

## Redis의 기타 자료형

- Stream: 로그를 저장하기 위한 자료구조
- Geospatial Indexe: 위도 와 경도 저장을 위해서 존재하고 이를 이용해서 거리 계산이 가능
- Bitmap: 비트 열
- Bit field: 비트를 나누어 놓은 열
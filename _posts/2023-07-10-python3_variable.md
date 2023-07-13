---
title: "Python (3)"
excerpt: "파이썬의 변수와 데코레이터"

categories:
  - python
tags:
  - python
last_modified_at: 2023-07-10
---

## **중첩함수** ##

- 파이썬은 함수 내부에 함수 만들 수 있음
- 내부 함수에서는 외부 함수 데이터 접근(읽기) 가능↔ 외부함수는 내부함수 데이터 접근 불가
- 내부 함수에서 외부 함수를 수정 불가
- 내부 함수는 함수 외부에서 호출 불가
- 파이썬 함수는 기본적으로 함수 외부 데이터에 직접 접근이 불가→ 내부 변수는 무조건 지역 변수로 생성

## **파이썬 함수 호출 로직** ##

- 먼저 **함수 해석** 후⇒stack, 코드를 줄 단위로 실행
- 함수 해석:
    - 함수 내에서 생성하는 변수 먼저 찾아서 지역 변수로 인식
    - 나머지 변수는 외부 변수로 인식


## **파이썬 변수** ##

#### global ####
- 함수 외부에서 생성, 전역 사용
    
#### member=receiver ####
- 클래스 내부에서 생성, 특정 인스턴스가 사용
- self.xxxxx ⇒ non-local, in class
- super.xxxx ⇒ 상위 클래스
    
#### local ####
- **함수 내부**에 생성, 내부에서만 사용
- c, java는 { }가 로컬 기준

## nonlocal과 global ##

- 함수 내부에서 외부 데이터 사용하기 위해 필요
- 함수 내부에 nonlocal이나 global이라는 키워드와 함께 변수 이름을 기재하면 함수 내부에서 해당 변수로 지역변수를 만들 수 없음
- nonlocal은 자신의 외부 함수에서 데이터를 찾고, global은 최상위에서 찾음
- 변수 이름 겹치지 않게 해야 함 → 예전에 g_, m_, _ 사용


## **Closure** ##

- 함수 내부 함수를 리턴해서 함수 외부에서 함수 내부 데이터 변경 목적으로 사용
- 객체 지향에서는 전역 변수를 권장하지 않음⇒ closure 이용하여 함수 내부-외부 데이터 공유

    ```python

    ## closure 예제

    def outer():
        outer_data=0

        #외부 함수의 데이터를 수정하는 내부 함수
        def inner():
            nonlocal outer_data
            outer_data+=1
            print(outer_data)
        return inner

    closure=outer()

    ```

## **프로그래밍 로직** ##

- business logic: 업무 관련
    - 기능적
- common concern: 업무 비관련  **⇒ decorator 필요성**
    - overhead?
    - 비기능적
    - 로그인, 로깅

    ```python
    def commonconcern1():
    print("common concern 1")

    def commonconcern2():
        print("common concern 2")

    def businesslogic():
        print("business logic 1")
    def transaction():
        commonconcern1()
        businesslogic()
        commonconcern2()
    
    ```

## **데코레이터(=intercepter) ⇒ for AOP (aspect of program)** ##

- 비즈니스 로직을 수행하기 전이나 후에 해야 할 일을 메서드로 만들어 두고, @함수이름으로 대신하도록 함
- business logic/common concern 분리하거나 생성 작업이 복잡하거나 알 필요 없는 경우 사용
- 로깅이나 벤치마크위한 테스트 코드, 유지보수 등  업무로직 방해하지 않고 수정
- 프록시 패턴
    - 개발자가 작성한 코드 대신 다른 코드를 불러내는 방식
- decorator 만들 때, 함수 리턴해서 리턴한 함수가 수행되도록 함
- decorator에 전달된 매개변수를 이용하여 함수 이름이나 인수, 리턴 확인 가능함
- 함수 호출할 때 마다 실행시간, 인수, 매개변수 , 리턴 값 출력
- functools의 [lru_cache()](https://velog.io/@ddyy094/LRULeast-Recently-Used-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%9D%B4%EB%9E%80) 데코레이터 사용하면 중복 함수 호출 줄일 수 있음 (@functools.lru_cache( ))

    ```python
    def deco(func):
    def wrapper():
        print(func.__name__, '함수 시작')  # __name__으로 함수 이름 출력
        func()  # 매개변수로 받은 함수를 호출
        print(func.__name__, '함수 끝')
    return wrapper
    @deco
    def businesslogic2():
        print("business logic 1")

    businesslogic2()

    ```

    ```python
    ##데코레이터 2
    import time
    def clock(func):
        #decorator 호출 시 수행될 함수
        def innerclock(*args):
            start = time.time()
            result=func(*args)
            end = time.time()
            elapsed = end - start
            print("수행시간: ",elapsed) #수행시간
            print("매개변수: ",args) #매개변수
            print("리턴값: ",result) #리턴값
            return result

        return innerclock

    @clock
    def koreanAge(age):
        return age+2

    print(koreanAge(25))

    ```
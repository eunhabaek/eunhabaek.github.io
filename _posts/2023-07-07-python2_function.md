---
title: "Python (2)"
excerpt: "파이썬의 함수"

categories:
  - python
tags:
  - python
last_modified_at: 2023-07-07
---

## Pure function (순수함수) ##
- 함수 내부에서 함수 외부 데이터 변경 않고 매개변수 동일하면 항상 동일한 결과를 리턴하는 함수
- 현재 시간, 랜덤 등 사용 불가

## 함수 도움말 확인 ##
- help 했을 때 출력되는 문자열 만들기
- 함수 만든 후 함수.__doc__ 속성에 문자열 대입 혹은 함수 가장 상단에 문자열 추가하면 help에 출력

    
## 재귀함수 (recursion, recursive call) ##
- 함수가 자기 자신을 다시 호출
- 재귀함수는 stack 여러개 동시 존재하므로 지양
- 반드시 종료점 필요
- 재귀는 함수 종료 전 자신을 다시 호출하므로 메모리 사용 많음, 스택 계속 생김
- memorization:
    - 함수 호출 결과 저장 후 재사용
    - import functools, @functools.lru_cache()하면 동일한 함수를 1번만 계산

    ```python
    #피보나치 수열 재귀로 =>메모리도 많이쓰고 오래걸림
    import functools
    @functools.lru_cache()

    def fibonacci(n:'int>=1'=0)->int:
        fibonacci.__doc__ = "재귀 이용하여 피보나치 수열 값 리턴하는 함수"
        if n==1 or n==2:
            return 1
        return fibonacci(n-2)+fibonacci(n-1)

    print(fibonacci(5))

    help(fibonacci) #함수 도움말 확인
    ```


## 함수 내 pass 사용 ##
- 함수나 클래스의 내용을 나중에 작성하기 위해 비워두는 것
- 애자일, TDD에 중요
        
    ```python
    def temp():
        pass
    ```

## 파이썬에서 함수는 일급 객체 ##
- 자료형
- 동적으로 생성 가능
- 변수에 대입 가능
    - overriding
    - 다형성
- 함수의 매개변수로 사용 가능
- 함수에서 리턴 가능


## lambda 람다 (=익명함수) ##
- 이름이 없는 한 줄짜리 함수→ 호출할 떄 만들어짐
- 변수 생성 불가, 할당문 및 while, try 불가
- lambda 매개변수 나열: 리턴 내용
    - ex) lambda x,y:x+y
- 주로 함수형 프로그래밍에서 함수의 매개변수


## **함수형 프로그래밍 관련 함수** ##

#### 1. **map** ####
- 두가지 의미
    - 이름과 값, 헤시테이블 등 dict( )
    - 변환한다는 의미
- 매개변수 1개 + 함수 ⇒결과 모아서  1개 데이터 리턴
- 형변환 (문자 →숫자)
- 반복문보다 속도 빠름
    
    ```python
    #문자열 3글자 이상인 부분 ... 처리하기
    animals=["cat","dog","rabbit","monkey"]

    def ext(x):
        if len(x)>3:
            return x[0:3]+"..."
        return x

    results=list(map(ext,animals))
    print(results)

    ```

#### 2. **filter** ####
- 매개변수 1개 +bool 리턴 함수⇒ 결과 모아  bool 리턴

    ```python
    #리스트에서 결측치 제거 한 후 문자열 길이 >3 만 추출하기

    colors=["red","orange","pink","blue",None]

    #결측치 확인
    print(None in colors)

    #결측치 제거
    def no_na(x):
        return x !=None

    #이름 3> 만 추출

    def len3(x):
        return len(x)>=3


    colors0=list(filter(no_na,colors))
    result=list(filter(len3,colors0))

    #lambda 이용 실습
    colors0=list(filter(lambda x:x!=None,colors))
    result2=list(filter(lambda x:len(x)>=3,colors0))
    ```

#### 3. **reduce** ####
- 기본 내장함수 아님, functools 패키지
- **매개변수 2개** 연산⇒ **하나의 결과**로 리턴
- 결과 리턴해서 계속 **누적 연산** 진행

    ```python
    from functools import reduce

    result=reduce(lambda x,y:x*y,[1,2,3,4])
    print(result)
    ```

#### 4. **zip** ####
- python 특이적 함수
- 여러 데이터 모임 받아서 하나의 tuple 데이터로 묶어줌
- 묶을 데이터 모임끼리 데이터 개수가 일치해야 함
    
    ```python
    school=["초등학교","중학교","고등학교"]
    school_name=["광주","탄벌","양서"]

    print(list(zip(school,school_name)))
    print(dict(zip(school,school_name)))
    ```

## **high order function 고위함수** ##

- 함수를 매개변수로 받거나 함수를 리턴하는 함수
- 파이썬에서는 함수 안에서 함수 생성 가능
- 함수 호출 결과를 변수에 대입한 후에 다시 호출 필요
- filter, map, reduce 등

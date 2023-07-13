---
title: "Python (1)"
excerpt: "파이썬의 리턴과 매개변수"

categories:
  - python
tags:
  - python
last_modified_at: 2023-07-07
---


## Return ##
- 함수 호출한 곳으로 돌아감
- 호출 시 데이터 가져갈 수 있음
- 파이썬은 여러개 데이터 , 로 나열하면 tuple로 간주
- return 없으면 매개변수 등 다른 데이터를 수정
    - ex) list.sort( )
- for 안에서 break와 비슷하게 사용 가능


## 매개변수(인수, 인자, argument, parameter) ##
- 함수 호출 시 함수에 넘기는 데이터
- 매개변수 있는 함수에 전달 안하면 에러
- 매개변수 자료형 명시 권장
    - ex) def add(a:int,b:int)-> int:

    ```python
    #매개변수 자료형 기입, UML 표기법
    def add(a:int,b:int)-> int:
    return a+b
    ```


## 매개변수 전달 방법 (call by value call by reference) ##
- scala data는 값이 전달, vector data는 참조 전달
- 함수 호출 시 scala 데이터 넘겨주면 함수 내부 데이터 변경되어도 외부 테이터 변경 X, vector 데이터 넘겨주면 함수 내부에서 함수 외부 데이터 수정 가능

## 매개변수 기본 값 설정 가능 ##
- 매개변수이름=기본값
- 매개변수 생략 시 적용되는 값
- 함수 호출 시 매개변수 이름과 값을 같이 전달 가능
## 매개변수의 unpacking ##
- 매개변수 여러개일때 list, tuple, set 등 한꺼번에 전달 가능 (*데이터)
- 전달된 list, tuple, set 분할해서 매개변수에 대입
- dict 전달시 *는 key 대입, value 전달은 ** 붙이기
- dict 전달 시 key 이름과 매개변수 이름 동일해야 함

    ```python
    # 매개변수의 unpacking
    def collect(a,b):
        print(a)
        print(b)

    col_dict={"a":10,"b":20}
    #key 전달
    collect(*col_dict)

    #value 전달
    collect(**col_dict)

    ```

## 가변 매개변수 ##
- 매개변수의 개수를 정하지 않고 사용하는 매개변수
- 매개변수 만들 시 * 이용하여 이름 설정
- 함수 내부에서는 매개변수들 모아 하나의 tuple 생성
- 가변 매개변수 앞에 있는 매개변수 대입시 매개변수 이름과 함께 대입하면 에러
- 가변 매개변수 뒤에 있는 매개변수 대입 시에는 반드시 매개면수 이름과 함께 대입해야 함
- **는 dict로 인식
- 함수 호출 시 존재하지 않는 매개변수 **이용해 대입하면 dict, dict는 가장 마지막 매개변수로

    ```python
    def merge(arg1,*arg2,arg3,**arg4):
    print(arg1)
    for elem in arg2:
        print(elem,end="")
    print()
    print(arg3)

    for elem2 in arg4:
        print(elem2,arg4[elem2])

    merge(2,"1",20,"2",30,arg3=4,a=1,b=3)
    ```

## 함수의 매개변수에 설명 추가 가능 ##
- def 함수이름(매개변수이름: ‘문자열’자료형) →리턴 타입:
- def pan(score:’int≥0 and ≤100’=0)→int:
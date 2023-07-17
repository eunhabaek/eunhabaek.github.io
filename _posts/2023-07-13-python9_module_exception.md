---
title: "Python (8)"
excerpt: "파이썬의 예외처리 "

categories:
  - python
tags:
  - python
last_modified_at: 2023-07-13
---

## 오류의 종류 ##
1. 컴파일 오류
    - 문법적 오류로 실행되지 않음
2. 예외 (exception)
    - 문법적인오류는 없어 실행은 되나,  실행 도중 멈춤
3. 논리적 오류
    - 문법적 오류 없어 실행은 되나, 결과가 이상함
4. 단언 (assertion)
    - 의도적으로 오류를 발생시켜서 프로그램을 중단 시키는 것

## 오류 발생 시 조치 ##
1. 컴파일 오류
    - 에러 찾아 직접 수정
2. 예외 (exception)
    - 수정할 수 있으면 수정을 하고 그렇지 않으면 예외처리
3. 논리적 오류
    - 알고리즘 문제이므로 디버깅 통해 해결 필요
4. 단언 (assertion)
    - 오류 아님

## debugging (디버깅) ##
- 예외나 논리적 오류를 찾아 해결 위해 수행하는 동작
- ide가 제공해주는 디버깅 툴 이용하여 메모리 확인
- 로그 출력하여 직접 확인
- 테스트 툴 이용

## 예외처리의 목적 ##
- 예외가 발생하더라도 프로그램을 정상적으로 동작시키기 위한 것
- 예외 내용을 로깅⇒ 확인 가능, 학습용으로 좋음

## 예외처리 형식 ##
try:
        예외 가능성 있는 구문
except:
        예외 발생 시 처리 할 구문
(else:
        예외 발생되지 않은 경우 수행
finally:
        예외발생 여부 상관없이 수행⇒외부자원 연결 해제, finally 나올 시 예외처리구문 종료)

```python
#예외처리 try except
def div(x):
    return 10/x

try:
    print(div(20))
    print(div(0))
except:
    print("예외 발생!")
print("프로그램 종료")
```  

#### 특정 예외 처리 ####
- except 예외클래스명 (as 변수명) : 변수 출력시 예외 확인 가능

#### 강제로 예외 발생 ####
- raise 예외클래스명(메세지):
    - 현재 함수 내에서 예외 처리
- raise
    - 상위 함수(호출한 함수)로 예외를 throw
        ```python
        ##예외 처리 2
        arr=list(range(0,40,10))
        try:
            number=int(input("나눌 정수를 입력하세요: "))
            lens=len(arr)
            i=0
            while i<lens:
                if number==1:
                    raise ValueError("강제 예외발생") # 1 입력 시 강제로 예외 발생
                assert number!=2, "2는 안됩니다." #2 입력 시 assertion 진행
                print(arr[i]/number)
                i+=1
        except ValueError as ve: #정수 입력하지 않을 시 예외처리
            print(ve)
            print("잘못된 데이터입니다. 정수를 입력하세요.")
        except ZeroDivisionError as zde: #0 입력 시 예외처리
            print(zde)
            print("0이 아닌 정수를 입력하세요.")
        else:
            print("예외 발생하지 않음")
        finally:
            print("프로그램 종료")
        ```


#### 예외 클래스 계층 확인 ####
-[예외 클래스 확인 주소](https://docs.python.org/3/library/exceptions.html)

#### Assertion ####
- 강제로 프로그램 중단
- assert 프로그램 중단되지 않는 조건[, 에러메세지]
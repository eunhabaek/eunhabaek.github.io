
---
title: "Python (4)"
excerpt: "파이썬의 클래스와 인스턴스"

categories:
  - python
tags:
  - python
last_modified_at: 2023-07-10
---

## Class 생성 ##
- class 클래스명:
    초기화 메서드 생성
    메서드 생성

  ```python
  class Student:
    class_data="클래스 속성" #클래스 속성 만들기

    #메서드 만들기
    def disp(self):
        print("인스턴스 생성")

    #속성 생성
    def setName(self,name):
        self.name=name #self.name은 인스턴스의 속성
  ```

## Instance 생성 ##
- 인스턴스 이름=초기화매서드(매개변수) ⇒ 초기화 메서드 이름은 클래스명
- 인스턴스 생성하는 메서드 호출하면 인스턴스 생성 후 참조를 리턴

  ```python
  #인스턴스 생성
  stu=Student() #재사용 용이
  ```

## 인스턴스나 클래스 이용한 호출 ##
- 인스턴스/클래스명.속성
- 인스턴스/클래스명.메서드명(매개변수)

## 메서드 호출 ##
- def 메서드명(인스턴스 참조 저장할 매개변수=self[,매개변수 나열]):
        내용
    
- 호출법
    1. unbound 호출⇒ 인스턴스 X, 일반적
      - 클래스명.메서드명(인스턴스, 매개변수)

        ```python
        Student.disp(stu)
        ```  
          
    2. bound  호출 ⇒인스턴스 O
      -  인스턴스.메서드명(매개변수)

        ```python
        stu.disp()
        Student().disp()  
        ```
    3. 클래스 내부
      - self.메서드명(매개변수)


## Instance 속성 만들기 ##
- bound 호출 메서드 안에서 self.속성명으로 데이터를 대입해 생성
- 속성 존재하면 수정, 없으면 생성
- 메서드 안에서 “self.”이용하지 않고 속성 만들면 지역변수

  ```python
  stu.setName("eunha")

  print(Student.class_data) #클래스명으로 클래스 속성 접근
  print(stu.class_data) #인스턴스명으로 클래스 속성 접근

  ```


## Class 속성 만들기 ##

- class 내부, method 외부에 변수 생성하여 데이터 대입하면 class 속성 생성
- class 속성은 class와 인스턴스 모두 접근 가능
- 인스턴스 이용해 접근 시 그 속성이 인스턴스 내부에 존재하면 인스턴스 속성을 호출
- 대입문 이용하면 인스턴스 속성 생성하여 사용
- 클래스 속성은 인스턴스 이용해서 접근하는 것 지양

  ```python
  #클래스명으로 클래스 속성 수정
  Student.class_data="클래스 데이터 수정"
  Student.class_new_data="클래스 데이터 생성"
  print(Student.class_data)
  print(stu.class_data)
  ```

  ```python
  #인스턴스명으로 클래스 속성 재수정
  stu.class_data="클래스 데이터 재수정"
  print(Student.class_data)
  print(stu.class_data) #인스턴스 안에 새로운 속성 생성
  ```

## **특수 목적 속성** ##

- __속성명__
- __doc__: 함수/클래스 설명
- __name__: 함수/클래스 명


  ```python
  ```

## **클래스의 속성명 생성 제한** ##

- class 의 __slots __ 에 속성 이름을 list로 나열하면 이 속성만 생성 가능


  ```python
  ```

## **인스턴스가 접근할 수 없는 속성 - private** ##

- __속성명 으로 생성 시 인스턴스가 접근할 수 없는 속성
- 속성의 직접 수정을 막기 위해 이용


## is 연산자 ##

- 파이썬은 데이터를 먼저 저장하고 그 데이터의 id를 저장하는 개념
- ==연산자는 id를 비교하는 것이 아님
    - __e__ 메서드를 호출해서 결과 리턴
    - ==는 오버로딩이 가능하여 어떻게 동작할 지 예측 어려움
- id를 비교하는 연산자는 is 연산자
    - 메서드 호출하지 않으므로 더 빠름
    - is는 오버로딩이 불가하여 변동 없음


  ```python
  #is 연산자 예제

  #인스턴스 생성해 대입
  stu1=Student()
  stu2=Student()

  #stu1의 데이터를 대입 - stu1이 참조하고 있는 데이터 참조를 같이 참조
  stu3=stu1

  #인스턴스 동일성 확인
  print(stu1==stu2) #내부 데이터 같은 지 확인 : False (인스턴스 별도 공간에 저장)
  print(stu1 is stu2) #id 같은 지 확인 : False

  print(stu1==stu3) #내부 데이터 같은 지 확인 : True
  print(stu1 is stu3) #id 같은 지 확인 True

  ```


---
title: "Python (5)"
excerpt: "파이썬의 메서드"

categories:
  - python
tags:
  - python
last_modified_at: 2023-07-11
---


**Accessor 접근자 메서드**

- 객체지향에서는 인스턴스로 속성에 직접 접근/수정을 지양⇒ (인스턴스.속성 X)
- 속성 접근 메서드 이용하여 속성의 값을 가져다 사용 후 수정함
- 접근자메서드 종류
    - getter (속성 값 리턴 메서드)
        - 이름을 “get속성이름”
        - 속성이름의 첫글자는 대문자(_사용하기도 함)
        - 매개변수 없고 속성을 리턴만 하면 됨
        - 속성 자료형 boolean이면 get 대신 is 사용 (판별)
    - setter (속성 값 생성/수정 메서드)
        - 이름은 “set속성이름”
        - 속성이름의 첫글자는 대문자
        - 매개변수 1개, 매개변수 값을 속성에 대입
        - 내용 작성하는 경우 대부분 유효성 검사
    - 속성의 자료형이 sequence(list)일 경우
        - 인덱스를 받아서 인덱스번째 데이터를 리턴하는 getter를 추가로 생성하고 인덱스와 데이터 1개 받아 인덱스번째 데이터 수정하는 setter 생성


  ```python
    #getter, setter 예제
    #이름과 점수 갖는 객체 여러개 필요
    class NameScore:
        def getName(self):
            return self.name

        def setName(self,name):
            self.name=name

        def getScore(self):
            return self.score

        def setScore(self,score):
            self.score=score

    namescore=NameScore()

    #setter 이용한 속성 생성
    namescore.setName("eunha")
    namescore.setScore(10)

    #getter 이용한 속성 사용
    print(namescore.getName())
    print(namescore.getScore())
  ```




## **Property** ##
- 속성과 유사하나 다름
- 속성을 조작하는 메서드를 변수에 저장해서, 속성을 조작하는 것 처럼 보이지만 실제로는 메서드를 호출하도록 만듦
- property명=property(fget=None, fset=None, fdel=None,  doc=None)
- ⇒ decorator 이용해서도 생성 가능


  ```python
  ```

**생성자와 소멸자**

- constructor (생성자)
    - __init__  (고정)
    - 인스턴스 생성 시 호출하는 메서드
    - 직접 생성하지 않아도 메모리 할당 하고, 참조 리턴하는 매개변수가 self만 있는 생성자 제공
    - 직접 생성자 만들면 제공되는 생성자 소멸
    - 생성자 만드는 이유: 처음부터 속성을 만들거나 속성을 원하는 값으로 초기화
    - 매개변수 이용해서 초기화 시 기본 값 없는 경우 매개변수 필수 입력 필요함
- destructor (소멸자)
    - 인스턴스 소멸 시 호출되는 메서드
    - __del__
    - 소멸자는 매개변수 self 이외에 불가
    - 소멸자 만드는 이유: **외부 자원**의 정리 (파일 핸들링, 네트워크, DB⇒ 정리 및 예외처리 필요함)
    - 인스턴스의 소멸:
        - 인스턴스 차지하던 공간은 garbage collection이 정리
        - reference count라는 개념으로 정리 대상 결정
            - 한 공간을 참조하는 변수의 개수
            - 참조하던 변수 소멸/null 대입 시 감소
            - count=0 이면 정리 대상
            - sys.getrefcount(인스턴스명) 으로 확인 가능 →  1 증가되어 나옴
            

**Static method, class method**

- 클래스 이용해서 호출하는 메서드
- 인스턴스 생성 없이 사용하기 위한 메서드
- 클래스 속성에만 접근 가능함
- static method
    - @staticmethod 라는 decorator 이용해서 생성
    - self 필요없음
    - 인스턴스가 소유하는 속성 접근 불가
- class method
    - @classmethod 라는 decorator 이용해서 생성
    - self 필요없음
    - class 인스턴스 대입받기 위한 매개변수 1개 필요
    - 인스턴스가 소유하는 속성 접근 불가

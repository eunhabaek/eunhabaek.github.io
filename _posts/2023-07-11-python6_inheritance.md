---
title: "Python (6)"
excerpt: "파이썬의 상속과 특별한 기능"

categories:
  - python
tags:
  - python
last_modified_at: 2023-07-11
---

## Operator overloading ##

- 제공되는 연산자의 기능을 변경하여 사용
- 파이썬은 각 연산자에 오버로딩 가능한 메서드 제공
- 매개변수 개수 변경은 불가
- +: \_\_add\_\_ , =: \_\_eq\_\_

    ```python
    #속성 생성제한, 연산자 오버로딩
    class Blank:
        #name과 age, __no 속성만 사용 가능하도록
        __slots__=["name","age","__no"]
        def __init__(self,name):
            self.name=name
            self.__no=1 #private 속성, 메서드 통해서만 접근 가능

        #연산자 overloading
        def __add__(self,other):
            return self.name+other.name

        def __eq__(self, other):
            return self.name==other.name

    bnk1=Blank("eunha")
    bnk2=Blank("eunha")
    bnk3=bnk1

    print(bnk1+bnk2) # 오버로딩 하지 않으면 에러

    print(bnk1==bnk2) #원래 false이나, 오버로딩으로 인해 true
    print(bnk1 is bnk2) #false

    print(bnk1==bnk3) #true
    print(bnk1 is bnk3) #ture    

    ```

## Inheritance (상속) ##
- 상위 클래스(super, base)의 모든 것을 하위 클래스(sub, drived)가 물려 받는 것
- 다중 상속 지원
- 상속 사용 이유:
    - 공통 내용을 상위 클래스로 추상화
    - 프레임워크에서 제공하는 클래스의 기능이 부족해서 확장
- 상속 방법
    - class 클래스 이름(상위 클래스 이름 나열):
- 하위 클래스에서 상위 클래스 method 호출
    - \_\_init\_\_ 함수는 super( ).init( )으로 호출
    - 하위 클래스에 \_\_init\_\_ 있으면 상위클래스 \_\_init\_\_ 호출하지 않음⇒ 인스턴스가 상위 클래스의 속성에 접근하고자 하는 경우 init 하지 않으면 에러 발생(상위 클래스의 속성을 만들기 않기 때문)
    - \_\_init\_\_ 이외의 함수는 super( ).method명으로 호출
- 메소드 overriding =재정의
    - 상위 메소드를 하위 클래스에서 다시 정의
    - 상위 클래스의 기능을 확장
    - 하위 클래스에서 메서드 만들 때 상위 클래스의 메서드 호출하는 것이 일반적
    - 프레임 워크에서 제공하는 클래스의 메서드를 오버라이딩 할 때 프레임워크의 메서드 호출하지 않으면 에러 발생하기도 함
- 다중 상속 ⇒ 지양
    - 여러개 클래스로부터 상속받음
    - 다중 상속 시 메서드 찾는 순서
        - class명.mro( )
    - 다중상속 시  super는 mro에서 첫번째 클래스의 인스턴스를 의미하는데 다른 클래스의 인스턴스를 사용하고자 하는 경우는 super(클래스명,self)를 대입

    ```python
    #상속 예시

    class Sup:
        def __init__(self):
            self.name="noname"
        def method(self):
            print("상위 클래스의 메서드")

    class Sub(Sup):
        #하위 클래스에서
        def __init__(self):
            super().__init__() #상속에서 중요함
            self.score=80
        def method(self): #메서드 오버라이딩
            super().method() #상위 클래스의 메서드 호출
            print("메서드 오버라이딩")

        def subMethod(self):
            print("하위 클래스의 메서드")

    #sub의 인스턴스 생성해서 필요한 메서드 호출
    sub=Sub()
    sub.subMethod()
    sub.method()
    print(sub.name)
    ```

- abstract(추상)
    - 추상 메서드
        - 내용 없고 이름만 존재, 하위 클래스에서 구현하여 사용
        - 메서드 상단에 @abstractmethod 추가
    - 추상 클래스:
        - 추상 메서드를 소유한 클래스
        - **본인의 인스턴스 생성 불가**
        - 상속 통해서만 사용하는 클래스
        - import abc 후, class (metaclass=abc.ABCMeta)
        - 만드는 이유:
            - 템플릿 메서드 패턴 구현⇒Interface(Protocol)
            - 여러 클래스의 공통 인터페이스, 명칭 생성


    ```python
    #추상 클래스 생성
    import abc
    class AbstractClass(metaclass=abc.ABCMeta):
        #추상 메서드 - 내용 없음, 하위 클래스에서 구현하여 사용
        @abc.abstractmethod
        def method(self):
            pass

    #추상클래스 상속０
    class Sub(AbstractClass):
        def __init__(self):
            print('인스턴스 생성')
        def method(self): #추상 메서드 반드시 implementation 해야함
            print("추상 메서드 구현")

    #instance=AbstractClass() #추상클래스는 인스턴스 생성 불가
    instance=Sub() #추상클래스 implementation 하지 않으면 에러
    instance.method()
    ```

## 특별한 기능 ##

1. delegation
    - 존재하지 않는 메서드 호출 시 처리를 위임
    - \_\_getattr\_\_(self,name) 메서드 재정의
    - name에는 호출하는 함수가 전달
2. iterator
    - 내부 데이터를 순차(next)적으로 접근할 수 있도록 해주는 포인터
    - \_\_ite\_\_와 \_\_next\_\_ 메서드를 재정의하여 구현
3. enumerate
    - iterator 객체에 적용하면 인덱스와 데이터를 순차적으로 접근할 수 있도록 해주는 함수
    - 인덱스와 데이터 동시 사용
4. generator
    - iterator의 특수 형태로 yield 키워드 조건 이용하여 데이터 하나씩 리턴
5. coroutine
    - 함수 수행 중 다른 함수를 호출하여 결과 사용/작업을 이어서 수행하도록 해주는 함수
    - 함수끼리 주고받기 가능, hand-shake , 웹서버
    - 구독-게시

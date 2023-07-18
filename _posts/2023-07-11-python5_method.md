---
title: "Python (5)"
excerpt: "파이썬의 메서드"

categories:
  - basic
tags:
  - python
last_modified_at: 2023-07-11
---

## **Accessor 접근자 메서드** ##

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



## **생성자와 소멸자** ##

#### constructor (생성자) ####
- \_\_init\_\_  (고정)
- 인스턴스 생성 시 호출하는 메서드
- 직접 생성하지 않아도 메모리 할당 하고, 참조 리턴하는 매개변수가 self만 있는 생성자 제공
- 직접 생성자 만들면 제공되는 생성자 소멸
- 생성자 만드는 이유: 처음부터 속성을 만들거나 속성을 원하는 값으로 초기화
- 매개변수 이용해서 초기화 시 기본 값 없는 경우 매개변수 필수 입력 필요함
##### destructor (소멸자) ####
- 인스턴스 소멸 시 호출되는 메서드
- \_\_del\_\_
- 소멸자는 매개변수 self 이외에 불가
- 소멸자 만드는 이유: **외부 자원**의 정리 (파일 핸들링, 네트워크, DB⇒ 정리 및 예외처리 필요함)
- 인스턴스의 소멸:
    - 인스턴스 차지하던 공간은 garbage collection이 정리
    - reference count라는 개념으로 정리 대상 결정
        - 한 공간을 참조하는 변수의 개수
        - 참조하던 변수 소멸/null 대입 시 감소
        - count=0 이면 정리 대상
        - sys.getrefcount(인스턴스명) 으로 확인 가능 →  1 증가되어 나옴

    ```python
    #클래스 생성
    class Animals():

        #생성자 - 인스턴스 생성할 때 호출
        def __init__(self,name="noname"): #속성에 대입할 매개변수="기본 값"
            print("인스턴스 생성")

            #생성자 안에 속성 생성하면 처음부터 속성 소유 가능

            self.default="기본 값" #default 값은 "기본 값"으로 밖에 초기화 안 됨
            self.name=name #매개변수 이용해서 속성 다르게 대입 가능

        #소멸자 - 인스턴스 삭제될 때 호출
        def __del__(self):
            print("인스턴스 소멸")

        def display(self): #메서드의 첫번째 매개변수는 기본적으로 self이며 실제 호출할 땐 생략
            print("인스턴스 메서드 출력") #인스턴스 없이는 호출 불가


        anm=Animals("cat") #인스턴스 생성, 참조 카운트 1

        #메서드 호출
        Animals.display(anm) #unbound 호출
        anm.display() #bound 호출

        print(anm.name)

        anm=None #인스턴스 소멸되어 소멸자 호출
    ```        

## **Static method, class method** ##
- 클래스 이용해서 호출하는 메서드
- 인스턴스 생성 없이 사용하기 위한 메서드
- 클래스 속성에만 접근 가능함

#### static method ####
- @staticmethod 라는 decorator 이용해서 생성
- self 필요없음
- 인스턴스가 소유하는 속성 접근 불가
#### class method ####
 @classmethod 라는 decorator 이용해서 생성
- self 필요없음
- class 인스턴스 대입받기 위한 매개변수 1개 필요
- 인스턴스가 소유하는 속성 접근 불가

```python
#일련번호 인스턴스 생성
class Nums():
    # 클래스 속성
    auto_increment = 0

    # 클래스 속성과 생성자를 이용한 일련번호
    def __init__(self):
        Nums.auto_increment+=1
        self.no=Nums.auto_increment

    @staticmethod #인스턴스 만들 필요 없음
    def staticmd():
        print("static method")

    @classmethod
    def classmd(cls):
        print(cls.auto_increment)


num1=Nums()
print(num1.no) #1 출력

num2=Nums()
print(num2.no) #2 출력

#static method 호출
Nums.staticmd()

#class method 호출
Nums.classmd()
```


## **Property** ##
- 속성과 유사하나 다름
- 속성을 조작하는 메서드를 변수에 저장해서, 속성을 조작하는 것 처럼 보이지만 실제로는 메서드를 호출하도록 만듦
- property명=property(fget=None, fset=None, fdel=None,  doc=None)
- ⇒ decorator 이용해서도 생성 가능


    ```python
  #property 직접 작성
    class Prop1:
        def __init__(self,name="noname"):
            self.__name=name
            self.age=27

        #접근자 메서드
        def getName(self):
            print("name의 getter 호출")
            return self.__name

        def setName(self,name):
            print("name의 setter 호출")
            self.__name=name

        #property 생성
        #name 호출하면 getName 메서드 호출, name 값 대입하면 setName 메서드 호출
        #name=property(fget=getName, fset=setName)


    pro1=Prop1()

    print(pro1.age)
    print(pro1.__name)

    #property 이용한 getter setter 호출
    pro1.name="eunha"
    print(pro1.name)
    ```

    ```python
    #property decorator 이용
    class Prop2:
        def __init__(self,name="noname"):
            self.__name=name

        #접근자 메서드
        @property
        def name(self):
            print("name의 getter 호출")
            return self.__name
        @name.setter
        def name(self,name):
            print("name의 setter 호출")
            self.__name=name

    pro2=Prop2()

    #property 이용한 getter setter 호출
    pro2.name="eunha"
    print(pro2.name)
    ```
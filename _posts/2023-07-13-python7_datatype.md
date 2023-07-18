---
title: "Python (7)"
excerpt: "파이썬의 데이터 타입"

categories:
  - basic
tags:
  - python
last_modified_at: 2023-07-13
---

## 문자열 ##

#### 데이터로 문자열 생성 방법 ####
1. “%포맷(d, f, c=문자1개, s=문자열).”%(데이터 나열)
    - "ten=%d"%(number)
    - 포맷 자릿수 설정
    - %10d: 열자리 확보 후 출력, 부족하면 늘어남
    - %.3f: 소수 반올림하여 3자리까지
        ```python
        number=10
        string="ten=%d"%(number)
        ```
2. “데이터 순서: 서식”.format(데이터 나열)
    - “ten={0:d}”.format(number)
        ```python
        number=10
        print("ten={0}".format(number))
        ```
3. f”문자열{변수명:서식}”의 형태로도 가능
    - f”ten={number:d}”
        ```python
        number=10
        print(f"ten={number}")
        ```
#### str의 메서드 ####
- dir(str)로 확인가능
- count( ): substring 몇개인지
- find( ): index( )와 동일한데 못 찾으면 -1
    
    ```python
    #GCCG 찾기
    problem="GDKSFSGCCGCCGGGGASADGCCGCCGASDAGA"
    #위치 전부 출력
    #한번 포함되면 그 문자열은 빼고 찾기
    target="GCCG"
    #print(target.find(problem))
    pos=[]
    i=0
    while(i<len(problem)-3):
        if target.find(problem[i:i+4])!=-1:
            pos.append(i)
            i+=4
        i+=1
    print(pos)
    ```

#### 문자열 인코딩 ####
- 오픈소스에서 문제, latin1 default
- encode(default method=시스템 인코딩): 문자열을 인코딩 된 바이트로 변환
- decode( ): 바이트 코드를 문자열로 변환
- ord( ): 하나의 문자를 코드로 변환
- chr( ): 코드를 문자로 변환
- 시스템 인/디코딩 설정 확인:
    - sys.stdin.encoding, sys.stdout.endcoding
        
## 정규 표현식 (regular expression-RegExp, RegEx) ##

- 특정 규칙 가진 문자열의 집합을 표현하는데 사용하는 형식어
- 표현식은 단순한데 문법은 가독성 떨어짐
- 파이썬에서는 re 모듈로 제공⇒ 표준 모듈
    - re.match (정규표현식, 데이터)
        ```python
        import re
        #:이나 |를 ,로 치환
        targetstr="cat: dog: monkey| rabbit"
        result=re.sub("[:,|]",",",targetstr)
        print(result)
        ```
- 자연어 처리, 입력 받을 시 유효성 검사
    - HTML5에서는 input 객체 pattern 속성 사용


## Bytes ##
- 바이트의 배열
- 네트워크 전송(시스템 다르면 바이트로만 전송), 파일 입출력, 이미지 프로세싱
- b”코드나 문자열 나열”

## List ##
- 코테에서는 가장 중요! 자료구조
- 데이터 순서대로 저장하는 객체 집합
- 변경/삽입/삭제(del, =[]) 가능하나, 느림
- 인덱싱과 슬라이싱 지원
- 데이터의 참조를 저장
- 메서드 종류
    - append(데이터), insert(인덱스, 데이터 ), index(데이터), count(값), reverse( ), remove(데이터⇒찾을 데이터), pop(인덱스⇒생략시 마지막)
- 리스트의 sorting
    - sort 함수 이용하면 Timsort(merge+insertion sort), 원본 수정

    - sorted 함수에 list 대입하면 리턴
    - key 속성에 함수 대입하면 함수 결과 리턴
    - reverse=True 내림차순
        ```python
        lst1=["cat","dog","rabbit","monkey","fox"]

        #데이터 추가
        lst1.append("lion")
        lst1.insert(4,"mouse")

        #데이터 삭제
        del lst1[1] 

        #데이터 정렬
        lst1.sort(key=lambda x: x[1])
        ```
- list의 대안
    - 정수나 실수 여러개는 array가 값으로 저장하므로 더 효율적
    - python의 int나 float는 클래스
    - 양쪽 끝에 데이터 추가/삭제하는 경우는 collections.deque
    - numpy나 scipy가 제공하는 고급 배열, 행렬 연산을 사용하면 과학계산 편리
    - deque 모듈의 Queue, LifoQueue, PrioriyQueue
    - multiprocessionf 모듈의 JoinableQueue : 프로세스 간 통신 지원 큐
    - asyncio: 비동기 프로그램
    - heapq: 가변 시퀀스를 이용해 큐 구현


## **Tuple** ##

- 임의 객체들이 순서를 가지는 데이터의 집합
- 리스트와 유사, 접근 방법 동일
- 내부 데이터 변경 불가
- 리스트는 보통 같은 데이터 타입끼리 저장하나, 튜플은 record 개념이므로 다를 수 있음
- 정렬하지 않음
- 생성방법
    - (데이터1, 데이터2, …)
    - (1개 데이터,)

        ```python
        record=("eunha",27,"student")
        ```
- 리스트와 튜플은  unpacking 기능 제공
    - 여러 변수에 데이터 나누어 저장
        ```python
        #unpacking
        name, age, job1 =record

        *etc,job2=record
        ```
    - swap에 사용

        ```python
        a=10
        b=20
        a,b=b,a
        ```
- 데이터 모임 만들 때 클래스 말고 튜플 쓰기도 함
    - VO(variable object): 값으로만 구성된 객체, 데이터 표현
    - DTO(data transfer object): 데이터 전송에 사용하는 객체 VO에 serializable을 구현하면 됨, Domain 객체 또는 Structure(구조체)라고 하기도 함

        ```python
        #dto 역할을 수행하는 클래스 생성 -> 클래스 내 속성명 바꾸면 출력 시 문제 생김
        #code sense 동작 가능

        class Dto:
            def __init__(self,name="noname",tel="notel"):
                self.name=name
                self.tel=tel

        dt1=[Dto("eunha","2409"),Dto("crystal","5705")]

        #데이터 출력
        for data in dt1:
            print(data.name,data.tel)
        ```

- 테이블 형태의 데이터 구조 만들 때 tuple의 list를 이용하는 경우 있음
- tuple은 각 컬럼명 부여하지 못하나, collections.namedtuple 이용 시 가능

    ```python
    #namedtuple
    from collections import namedtuple
    students=namedtuple("students","name age")
    stu1=[students("eunha","27"),students("crystal","25")]
    for i in stu1:
        print(i.name)
    ```

## Set ##
- 데이터 순서 상관 없이 해싱 이용해 데이터 저장
- 중복 데이터는 저장 X
- set과 frozenset(객체 내용 변경 불가 set)
- 생성 방법
    - {데이터 나열} ⇒ { }는 빈 dict 만듦
    - set( 데이터 나열) ⇒비어있는 셋 만들기 가능
- 집합 연산을 위한 메서드
    - union, intersection, difference, symmetric_difference, update, intersection_update
- 데이터 존재 여부 확인 가장 빠름
- 순서 상관 없이 저장⇒ 출력 순서 예측 불가

    ```python
    #set 생성
    animals={"cat","cat","dog"}
    print(animals)

    #set 순회
    for i in animals:
        print(i)
    ```

## Dict ##
- key value 쌍으로 저장된 자료구조
- key가 set으로 만들어짐
- 생성
    - (key:value, key: value)
    - key 자료형 아무거나 가능하나, 특별하지 않으면 str
    - dict이름[키] 키가 존재하지 않으면 KeyError
- key 존재하지 않으면 기본값
- for 이용해 순회하면 key 순서대로 배열
- keys, values, items이용 데이터 추출 가능
- del 이용 부분삭제, claer 전체 삭제
- 삽입하거나 조회 시 O(1)
- 메모리 낭비 심함
- MVC
    - model, view,  controller
    - 모델이 변해도 모델 X

        ```python
        #dict를 이용한 mvc -> 속성명 바꿔도 출력에 문제 없음
        #but code sense 적용 안됨
        dt2=[{"name":"eunha","tel":"2409"},{"name":"crystal","tel":"5705"}]

        for data in dt2:
            for key in data:
                print(key,":",data[key])
        ```

        ```python
        #이차원 배열 대신 dict는 key 이용 접근
        array2=[{"name":"그룹1","data":group1},{"name":"그룹2","data":group2}]

        for dic in array2:
        print(dic.get("name"),end=": ")
        for elem in dic.get("data"):
            print(elem, end="\t")
        print()
        ```

## **List comprehension** ##

- 반복 가능한 객체 이용하여 새로운 반복 가능한 객체 축약하여 생성
- map, filter (with if)보다 빠름

    ```python
    #list comprehension - 두개 리스트 이용
    li1=[1,2,3]
    li2=[4,5,6,7]

    re=[x*y for x in li1 for y in li2]
    print(re)
    ```
    
    ```python
    animals=["cat","dog","fig","lion","rabbit"]
    
    #글자 수 4 이상 추출하기
    #list comprehension - with if
    re2=list(x for x in animals if len(x)>=4)

    #글자 수 4이상 5 미만 추출하기
    #list comprehension - with if 2개 또는 and, or 사용 가능
    re3=list(x for x in animals if len(x)>=4 if len(x)<5)
    
    #글자 수 4자리 이상 ...붙여서 출력  
    #list comprehension - with if else 문 활용
    re4=list(x if len(x)<4 else x[0:3]+"..." for x in animals)
    ```
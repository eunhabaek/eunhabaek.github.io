---
title: "Javascript (6)"
excerpt: "자바스크립트의 함수"

categories:
  - frontend
tags:
  - javascript
last_modified_at: 2023-07-03
---

## 1. 함수란? ##
- 별도의 메모리 할당받아 수행되는 코드 블럭
- 목적
  - 모듈화: 프로그램을 용도별로 분할
  - 메모리를 효율적으로 이용 (스택 1mb로 한정,static과 heap은 거의 무한)

## 2. 함수의 종류 ##
#### a. User defie function: 사용자 정의 함수 ####
- function 함수명(매개변수 나열){
    함수 내용
    return 데이터 //생략가능
    }
- 위의 내용 자체를 let. var 이용하여 변수로 할당 가능 ->최근 권장되는 방식, 순서 중요, 함수 먼저 작성

  ```javascript
  let f1=function(){
      document.write("안녕"+"<br/>")
  }
  f1() //별도 메모리 공간 할당받아 f1 실행
  ```
- 함수 만들면 static(이름 O)/heap(이름 X →rambda: 메모리 절약)에 올라옴
- 함수 호출=실행→ stack에 메모리 할당됨
- 함수 이름만 기재할 경우 함수를 참조하는 것

#### b. Maker function: 언어가 제공
#### c. 3rd party function: 다른 개발자가 만들어 제공
    

## 3. 매개변수(argument, parameter, 인자, 인수) ##

- 함수를 호출할 때 넘겨주는 데이터
- 개수 제한은 없고 , 이름만 기재
- javascript나 python은 매개변수 자료형 기재하지 않으므로 네이밍 중요
- 함수 호출 시 뒤에서 생략 가능 →undefined
- 매개변수 있는 함수를 매개변수 없이 호출하면 undefined

  ```javascript
  let intAddInt=function(a,b){
          let summ=a+b
          return summ
        }
  document.write(intAddInt(2,3)+"<br/>")

  //매개변수 뒤에서 생략 가능
  //생략하면 undefined 대입

  result=intAddInt(3) //리턴값 변수 할당하면 재사용 가능
  document.write(result+"<br/>")
    ```

## 4. Return ##

- 함수가 작업 수행하고 호출하는 곳으로 돌아가는 제어 명령어
- 데이터 리턴하면 그 데이터 재사용 가능
- python 데이터 전처리나 분석 함수는 보통 데이터 리턴
- 전처리 함수는 원본 수정 지양 → 리턴 값 이용
- 리턴 생략된 경우는 마지막에 return이 있는 것으로 간주
- 리턴 값 없으면 해당 블럭 이어서 진행 불가

## 5. 객체로서의 함수 ##

- 함수도 하나의 자료형으로 간주(함수도 하나의 데이터)
- 함수의 내용을 변수에 대입 가능, 매개변수로 사용 가능, 리턴 가능
- 함수를 변수에 대입하는 이유- 이벤트 처리, 다형성 구현을 위해 (Delegate)
- 함수의 내용을 다른 변수에 담아 호출하는 것을 delegate(위임)이라고 함

  ```javascript
  //객체로서의 함수
  function zergattack(){
      document.write("저그의 공격"+"<br/>")
  }

  function protossattack(){
      document.write("프로토스의 공격"+"<br/>")
  }
  
  //zergattack()
  //protossattack()

  let starcraft
  starcraft=zergattack //변수에 함수 대입
  starcraft()
  starcraft=protossattack
  starcraft() //다형성
  ```

- 함수를 매개변수로 받는 이유- 함수에 따라 수행하는 일을 다르게 하기 위해(map reduce programming-map, filter, reduce)
- 함수를 리턴하는 이유- 대부분 closure 구현 위해서 (customizing)
- closure: 함수 안에서 함수를 리턴하여 함수 내부 데이터를 외부에서 변경하는 것
   
  ```javascript
  //closure 예제 ->전역변수 안쓰도록
  function outer(){
      let data=0 //함수 안에서만 사용 가능한 변수

      return function(){
          data++
          console.log(data)
      }        
  } 

  let inner=outer() //outer에서 return 함수가 저장
  inner() //1
  inner() //2
  ```
    


## 6. Arrow function (익명함수) ##

- 함수를 이름없이 생성
- (매개변수)⇒{  
    함수 내용  
    }  
    
- 함수를 미리 메모리에 할당(static)하지 않고 필요할때 메모리 할당(heap) →메모리 절약하나 시간 오래걸림
- 일반적으로 이벤트(사용자나 시스템이 발생시키는 사건 ex. click…) 처리에 사용 (선택적 사용)

  ```javascript
  //arrow 함수
  const arrow = () => {
      alert("arrow function")
  }
  arrow()
  ```

## 7. 내장함수 ##

- 프로그래밍 언어가 제공
- javascript에서는 window 객체가 제공하는 함수가 내장함수 (window. 생략)
- 객체가 소유한 함수는 메서드, 메서드는 리시버와 함께 호출해야하는데 window 객체의 메서드는 리시버 생략하면 widow 객체의 메서드가 호출
- 리시버(클래스, 인스턴스…).메서드 형태
- 종류
  1. alert(대화상자에 메세지 출력)
  2. confirm(메세지)
    - 버튼 두개 (확인⇒true, 취소⇒false) boolean 값 리턴

      ```javascript
      let hungry=confirm("배고픈가요?")
      if (hungry){
          document.write("배고파요")
      }else{
          document.write("배고프지 않아요")
      }
      ```
  3. codec
    - encoding: 메모리에 저장되는 코드로 변환
    - decoding: 사람이 알아볼 수 있게 변환
    - utf-8 → 한글 표준
    - ms949(cp949)→ windows
    - euc-kr → 옛날 웹
    - iso-latin1(iso-8859-1) →서유럽 표준어

      ```javascript
      let iu="name=아이유"
      let encoding=encodeURI(iu)
      
      alert(encoding)//한글 깨짐
      alert(decodeURI(encoding)) //다시 한글 정상 출력
      ```
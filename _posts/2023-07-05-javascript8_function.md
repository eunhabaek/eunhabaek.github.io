---
title: "Javascript (8)"
excerpt: "자바스크립트의 method와 inheritance(상속)"

categories:
  - frontend
tags:
  - javascript
last_modified_at: 2023-07-05
---
## 메서드(method) 란? ##
- Function과 Method의 공통점 → 동적인 데이터
- Function은 전역 객체
- Method는 리시버(클래스=static, 인스턴스) 통해서만 접근가능
- 객체 자신 가르키는 숨겨진 포인터 존재(this)
- super는 상위 클래스 포인터

  ```javascript
  //함수에서 포인터 사용하기
  let obj={name:"eunha",
        disp:function(){
        document.write(this.name);
  }}
  obj.disp()
  ```

## 접근자 메서드 ##

1. getter (속성을 리턴하는 메서드)
  - 이름을 “get속성이름”
  - 속성이름의 첫글자는 대문자
  - 매개변수 없고 속성을 리턴만 하면 됨
  - 속성 자료형 boolean이면 get 대신 is 사용 (판별)
2. setter( 속성 값 수정 메서드)
  - 이름은 “set속성이름”
  - 속성이름의 첫글자는 대문자
  - 매개변수 1개, 매개변수 값을 속성에 대입
  - 내용 작성하는 경우 대부분 유효성 검사

```javascript
//getter setter
let human = class{
    constructor(name,score){
        this.name=name
        this.score=score
    }
    //print
    disp(){console.log(this.name+":"+this.score)}

    //getter
    getName(){return this.name}

    //setter
    setName(name){this.name=name}

    getScore(){return this.score}

    //유효성 검사 setter
    setScore(score){
        if(score<0||score>100){
            alert("적절하지 않은 점수입니다.")
            return
        }
        this.score=score
    }
}

```
## static 매서드 ##

- 인스턴스 아니라 클래스가 호출하는 매서드
- this 포인터 사용불가
- 함수 앞에 static 기재

```javascript
//인스턴스 매서드는 인스턴스 속성으로 호출 가능
let obj2 =new human("eunha",10)
obj2.setScore(100)
obj2.disp()
//인스턴스 없이 매서드 호출하려면 static 매서드 필요
```
        
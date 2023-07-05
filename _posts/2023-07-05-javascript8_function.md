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

## 상속 (Inheritance) ##
- Sub class extends Super{}
- 하위 클래스가 상위 클래스 모든 것 물려받음
- 같은 기능 같은 이름, 다른 기능 다른 이름
- 상속 관련 용어
  - 객체지향에서는 상속 관계 = is a
  - 상위 클래스: super, base
  - 하위 클래스: sub, drived
  - 단일 상속: 하나의 클래스로부터
  - 다중 상속: 두개 이상으로부터
- overriding(재정의)
  - 상위클래스 메서드를 하위클래스에서 다시 정의함 (이미 있는 메서드)
  - 추상 메서드(비어있음)를 다시 만드는 것은 재정의 아니고 구현(implementation)
  - 목적: 기능 확장
  - 상위 클래스 메서드를 호출해야 함
- Super
  - 하위클래스의 메서드에서 상위 메서드 호출시 super.메서드명

```javascript
//상속 예제
let Super = class{
    method(){
        console.log("상위 클래스의 매서드")
    }
}
//Sub 생성
class Sub extends Super{
    //overriding
    method(){
        super.method()
        console.log("하위 클래스의 매서드")
    }
}

let obj= new Sub()

//Sub에는 method 없지만 Super로부터 상속받아서 method 사용 가능함
obj.method()
```
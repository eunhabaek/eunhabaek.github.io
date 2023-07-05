---
title: "Javascript (7)"
excerpt: "객체 지향 프로그래밍"

categories:
  - frontend
tags:
  - javascript
last_modified_at: 2023-07-03
---

**OOP (objective oriented programming)**

## object (객체) ##
- 존재하는 모든 것
- 동일한 목적을 달성하기 위해 모인 속성(변수, 상수)과 메서드(함수)의 집합
- 인스턴스: 클래스 기반으로 만들어진 객체 → new
- 프레임워크(jdk)/>라이브러리(numpy)/>클래스/>인스턴스

## 객체 지향의 3대 성질 ##
- Encapsulation: 클래스와 인스턴스를 만드는 것
- Inheritance: 하위 객체가 상위 객체로 물려받는 것 (재사용)
- Polymorphism: 동일한 메세지에 대하여 다르게 반응

## 사용자 정의 객체 ##
- 객체는 이름으로, 배열은 순서로 찾음
- 이름으로 찾는게 쉬우므로 데이터 전달 시 데이터는 객체로 전달하는것이 좋음
- /[ /]: 배열, /{ /}: 객체
- 객체 생성방법
    1. let, const 이름 {속성이름: 데이터or 함수, … } 
        - 이름.속성이름 = 데이터 (upsert→기존 속성에 없으면 insert, 있으면 update)
        - delete 이름.속성이름
        - console.log는 속성 데이터 바로 보임

            ```javascript
            let ar1=[{name:"eunha",age:27},
                    {name:"sera",age:25},
                    {name:"sam",age:40}]
            for(let i in ar1){
                document.write(ar1[i].name+" "+ar1[i].age+"<br/>")
            }
            ```

    2. 생성자로 만들기
        - constructor(생성자)는 class의 일부
            - 생성자 호출법: new 생성자 이름(매개변수)
            - new → heap에 올림
            - 생성자로 인스턴스 만들때는 new와 함께 호출하면 heap에 인스턴스 생성
            - constructor 목적은 초기화
        - 생성자의 역할
            - heap 공간 할당
            - 내부 초기화

        ```javascript
        let person=function(name){
                    this.name=name
                    this.disp=function(){console.log(this.name)}
                }
        //매서드 말고 속성만 설정해서 인스턴스 생성
        //생성자로 인스턴스 만들때는 new와 함께 호출하면 heap에 인스턴스 생성
        ar3=[new person("eunha2"),new person("sera2")]

        for(idx3 in ar3){
            ar3[idx3].disp()
        }
        ```        
        
    3. 클래스를 만들고 생성하기 //가장 권장하는 방법
        - 클래스: 동일 인스턴스 만들기 위한 모형
        - 클래스는 첫글자 대문자로
        - 클래스 안에 매서드 만들때는 let이나 function 생략 가능
        - 형식: let 클래스명=class{  
            constructor(매개변수){  
            내용  
            }  
            매서드 나열  
            }  
            
        - 클래스 이용해서 인스턴스 생성
            - new 클래스이름(매개변수); //클래스 이름 적지만 실제로는 contructor 호출

        ```javascript
        //클래스 생성하고 인스턴스 생성
        //클래스는 첫글자 대문자로
        let Member=class{
            constructor(name){
                this.name=name
            }
            //클래스 안에 매서드 만들때는 let이나 function 생략 가능
            disp(){
                console.log(this.name)
            }
        }

        ar4=[new Member("tom"), new Member("lena")]
        for(idx4 in ar4){
            ar4[idx4].disp()
        }
        ```
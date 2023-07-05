---
title: "Javascript (9)"
excerpt: "자바스크립트의 built-in 객체"

categories:
  - frontend
tags:
  - javascript
last_modified_at: 2023-07-05
---

## Built-in 객체 란? ##
- javascript가 기본적으로 제공해주는 객체

## 일반 객체 ##
: 프로그래밍에 필요한 built-in 객체 

#### 1. Object: 최상위 객체
- 자바스크립트의 모든 객체가 상속 받음

#### 2. Math: 수학 관련 기능 제공
- 인스턴스 생성 X
- Math.속성 형태로 사용
- Java의 Math 클래스와 동일
- Math.land() →0.0-1.0 사이 랜덤값 리턴

#### 3. String
- 변경할 수 없는 문자열 저장
- 모든 메서드가 작업 후 리턴 (원본 수정 X)
- =으로 돌려 받아야 함

  ```javascript
  // string 예제
  //모두 대문자 변경, 좌우 공백 제거

  let dogname="   jindodog  "

  let upperdog=dogname.trim().toUpperCase()
  console.log(upperdog)

  //확장자 추출
  let filename="filename.hwp"
  let pos=filename.indexOf(".")
  document.write(filename.substring(pos))

  ```

#### 4. 배열 관련 함수 가진 객체

```javascript
//Array 예제
numar=[1,45,36,26,78,4,134]
```
- **length**: 배열 데이터 개수 파악

```javascript
//데이터 개수 파악
console.log(numar.length)
```
- **for in**: 배열 순회
- **forEach**:
- 배열.forEach((element, index, array)=>{  
    실행 내용})  
- 배열 순회하며 데이터 하나씩 접근

  ```javascript
  //forEach 순회하며 작업
  numar.forEach((element, index, array)=>{
      console.log(element)
  })
  ```

- **map**: 데이터 변환
- 사용 형태는 forEach와 동일하나, 함수가 데이터를 리턴해야 함
- 리턴한 데이터 모아서 다시 배열로 리턴
- Map-Reduce
  - 분산 데이터 처리 후 결과 모음
  - 병렬 처리 가능

  ```javascript
  //map 데이터 변환
  let numarmap=numar.map((element,index,array)=>{
      return element+1
  })
  console.log(numarmap)        
  ```          
- **filter**: 추출
- boolean 검사 후 추출

  ```javascript
    //filter
    let numarflt=numar.filter((element,index,array)=>{
        return element%2==1
    })
    console.log(numarflt)

    //
    let ls1=[{name:"Eunha",gender:"F",age:27},
            {name:"hain",gender:"F",age:40},
            {name:"amin",gender:"M",age:50}]

    let flst=ls1.filter((element,index,array)=>{
    return element.gender=="F"
    })
    console.log(flst)            
  ```
- **sort**: 정렬
- 데이터 순서대로 나열
- 오름차순 ascending (default)
- 내림차순 descending
- 크기비교 가능 데이터만 정렬 수행
- 리턴하지 않고 배열 직접 조작
- 자리수 다르면 그냥 sort 정렬 바르게 되지 않음
- 정렬 시 크기 비교 함수를 대입받아서 사용자 정의 정렬을 지원
    - 함수 모양은 매개변수 2개, 정수 리턴
    - 앞 데이터 크면 1, 두개가 같으면 0, 작으면 -1
- 객체끼리는 비교 불가
- 문자는 크기비교 가능
- 소문자가 대문자보다 큼

  ```javascript
    //sort 실패
    numar.sort()
    console.log(numar)

    //sort 성공
    numar.sort((left,right)=>{
        return left-right //right-left는 descending
    })
    console.log(numar)

    //리스트 sort
    lsag=ls1.sort((left,right)=>{
        return left.age-right.age
    })
    console.log(lsag)

    //이름순 sort
    ls1.sort((left,right)=>{
        if (left.name>right.name){return 1}
        else if (left.name<right.name){return -1}
        else{return 0}
    })
    console.log(ls1)

    alert(navigator.userAgent)

  ```

## BOM: (browser object model): 브라우저 관련 객체
- window나 navigator 객체
- window 안에는 내장 함수, timer
- navigator 객체의 userAgent 속성에 접속한 운영체제와 브라우저 확인 가능한 문자열

## DOM (document object model): body 태그 관련
- body에 만든 요소 찾기
- document.getElementByID(아이디명)

  ```html
  <body>
    <!--DOM-->
    <div id="display"></div>
    <input type="text" id="name"/>
    <button id="btn">버튼</button>

    <script>
      document.getElementById("btn").addEventListener("click",(e)=>{
        document.getElementById("display").innerHTML=document.getElementById("name").value
    })    
    </script>
  </body>
  ```

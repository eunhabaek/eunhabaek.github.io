---
title: "Javascript (4)"
excerpt: "switch문과 if문"

categories:
  - frontend
tags:
  - javascript
last_modified_at: 2023-07-03
--- 

## 1. switch 문 ##
- 값에 의한 분기 (언어마다 다르므로 학습할 때 주의하기)
- 값 = 정수, 문자열
- 형식: switch(정수나 문자열 표현식){  
    case값1: 표현식이 값1일때 수행 내용  
    case값2: 표현식이 값2일때 수행 내용  
    ..  
    default: 일치 값 없을 때 수행 내용  
    }  
- javascript에서 switch는 fallthrough가 기본이라 일치하는 값 만났을 때 종료하려면 break 필요

    ```javascript
    //메뉴 선택하기
    let menu=1
    let msg;
    switch(menu){
        case 0:
            msg='한식'
            break;
        case 1:
            msg='양식'
            break;
        case 2:
            msg='일식'
            break;
        default: '선택을 다시 해주세요'
    }
    console.log(msg3)
    ```

- 정수나 문자열 표현식에 true 설정하면 값의 자리에 조건 설정해서 true인 경우 수행하도록 할 수 있음

    ```javascript
    //switch를 if처럼 사용하기 -> if가 더 간결, 권장하지는 않음
    let score=88
    let msg2;
    switch(true){
        case score>=80&&score<100:
            msg2='A'
            break;
        case score>=60:
            msg2='B'
            break;
        case score>=0:
            msg2='C'
            break;
        default: '점수를 바르게 입력해주세요'
    }
    console.log(msg2)
    ```
- case 개수는 제한 없음
- default 생략 가능
    
## 2. if 문 ##
- 조건에 의한 분기 (true, false)
- 형식: if(조건식1){조건식1이 truthy 일 경우 수행되는 내용  
    }else if(조건식2){ 조건식1이 falsy이고 조건식2가 true일 경우 수행  
    }else{이전 모든 조건식 falsy일 경우 수행}  
    
- falsy: false, 0, null, undefiend, “”
- else if는 무제한 사용 가능
- 조건식은 변수, 연산, 리턴 있는 함수 호출

    ```javascript
    //year가 윤년이면 "윤년", 아니면 "평년"
    let year=2024;
    let msg3;
    if (year%4==0&&year%100!=0||year%400==0){
        msg3=year+"년: 윤년"
    }else if(year<0){
        msg3=year+": 올바른 년도가 아닙니다."
    }else{
        msg3=year+"년: 평년"
    }
    console.log(msg3)
    ```

## 3. if문이나 switch문 사용시 주의점 ##
- if(리스트로 찾음)보다 switch(해시로 찾음)가 빠름
- 도달할 수 없는 코드 작성→ 화이트 박스 테스트로 확인

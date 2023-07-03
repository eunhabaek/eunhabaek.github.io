---
title: "Javascript (4)"
excerpt: "switch와 if문"

categories:
  - frontend
tags:
  - javascript
last_modified_at: 2023-07-03
--- 

## 1. switch 문 ##
- 값에 의한 분기 (언어마다 다르므로 학습할 때 주의하기)
- 값 = 정수, 문자열
- switch(정수나 문자열 표현식){
    
    case값1: 표현식이 값1일때 수행 내용
    
    case값2: 표현식이 값2일때 수행 내용
    
    ..
    
    default: 일치 값 없을 때 수행 내용
    
    }
    
- javascript에서 switch는 fallthrough가 기본이라 일치하는 값 만나도 break 만날 때까지 계속 진행함
- 정수나 문자열 표현식에 true 설정하면 값의 자리에 조건 설정해서 true인 경우 수행하도록 할 수 있음
- case 개수는 제한 없음
- default 생략 가능
    
## 2. if 문 ##
- 조건에 의한 분기 (true, false)
- if(조건식1){조건식1이 truthy 일 경우 수행되는 내용
    
    }else if(조건식2){ 조건식1이 falsy이고 조건식2가 true일 경우 수행
    
    }else{이전 모든 조건식 falsy일 경우 수행}
    
- falsy: false, 0, null, undefiend, “”
- else if는 무제한 사용 가능
- 조건식은 변수, 연산, 리턴 있는 함수 호출

    ```html
    <script>
        //year가 윤년이면 "윤년", 아니면 "평년"
        let year=2024;
        let msg2;
        if (year%4==0&&year%100!=0||year%400==0){
            msg2=year+"년: 윤년"
        }else if(year<0){
            msg2=year+": 올바른 년도가 아닙니다."
        }else{
            msg2=year+"년: 평년"
        }
        console.log(msg2)
    </script>
    ```

## 3. if나 switch 사용시 주의점 ##
- if(리스트로 찾음)보다 switch(해시로 찾음)가 빠름
- 도달할 수 없는 코드 작성→ 화이트 박스 테스트로 확인

---
title: "Javascript (4)"
excerpt: "자바스크립트의 제어문"

categories:
  - frontend
tags:
  - javascript
last_modified_at: 2023-07-03
--- 

1. 분기
    - switch
        - 값에 의한 분기 (언어마다 다르므로 학습할 때 주의하기)
        - 값= 정수, 문자열
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
        
    - if
        - 조건에 의한 분기 (true, false)
        - if(조건식1){조건식1이 truthy 일 경우 수행되는 내용
            
            }else if(조건식2){ 조건식1이 falsy이고 조건식2가 true일 경우 수행
            
            }else{이전 모든 조건식 falsy일 경우 수행}
            
        - falsy: false, 0, null, undefiend, “”
        - else if는 무제한 사용 가능
            - 조건식은 변수, 연산, 리턴 있는 함수 호출
    - if나 switch 사용시 주의점
        - if(리스트로 찾음)보다 switch(해시로 찾음)가 빠름
        - 도달할 수 없는 코드 작성→ 화이트 박스 테스트로 확인

1. 반복
    - 반복:  횟수 지정함, index 필요, 유지보수 및 재사용 가능
        1. while
            - while(boolean 표현식){
                
                표현식이 falsy가 아니면 반복할 내용
                
                }
                
            - 표현식을 확인하면서 반복
        2. do ~ while
            - do{
                
                표현식이 falsy가 아니면 반복할 내용
                
                }while(boolean 표현식);
                
            - 표현식을 확인해서 반복
            - while과의 차이는 처음 수행 유무
        3. for
            - while과 유사한 용도, 객체나 배열 순회
            - for (처음만 수행되는 식;판별식을 위한 표현식;두번째부터 수행할 식){
                
                표현식이 falsy가 아닌 경우 수행할 내용
                
                }
                
            - 세가지 식은 생략 가능, 셋 다 없으(;;)면 무한반복
            - for에서 첫번째, 세번째 식에 여러개 수행문 작성 가능 “,”로 구분
        4. for ~ in
            - for(임시변수 in 배열이나 객체){
                
                수행할 문장
                
                }
                
            - 배열에서는 인덱스가 순서대로 임시변수에 대입, 개체의 경우는 속성이 임시변수에 대입
            - 
            
    - 순회 (iteration): 0개 이상의 데이터를 가진 collection을 순차적으로 접근, index 불필요
2. 기타
    - break: 반복문 종료
    - continue: 다음 반복으로 넘어가기
    - return
        - 현재 모듈 수행 종료
        - 호출 지점으로 돌아가기
        - 진행 지점 저장가능
        - return 없으면(void) 못돌아감

        
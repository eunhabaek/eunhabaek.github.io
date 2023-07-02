---
title: "Javascript (2)"
excerpt: "자바스크립트의 데이터"

categories:
  - frontend
tags:
  - javascript
last_modified_at: 2023-07-01
--- 

## 데이터 분류 ##
- 변경 여부
    - mutable data: 변경 가능,  동시 불가
    - inmutable data: 변경 불가, 동시 가능
- 데이터 타입
    - scalar data:  1개
    - vector data: 0개 이상 (data 없을 수도)
- 데이터 유형에 따른 분류
    - 정형:  구조 일정 - 이미지, 숫자
    - 비정형: 구조 비일정- 텍스트, 음성
    - 반정형:  메타데이터 포함하여 정형으로 파싱 가능 - xml, json, yaml
- 저장 종류에 따른 분류
    - value: 값을 저장 - go, c
    - reference: 메모리 참조 - python, javascript

## 데이터 네이밍 ##

1. 전역 데이터(global)로 만들기
    - 이름=값
2. 로컬 데이터 -자신의 영역에서만 사용 가능
    1. var 이름=값 //hoisting 가능, 값 변경 가능
    2. let 이름=값  //hoisting 불가, 변경 가능 - 권장
    3. const 이름=값 //값 변경 불가
3. 값 변경
    - 기존이름=새로운 값
4. hoisting
    - 이름 만들기 전에 사용 가능
        
## 데이터의 종류 ##
1. number
2. string
3. boolean
4. undefined: 변수 이름은 있으나 데이터 없을때
5. null: 가리키는 데이터가 없음
6. array 
    - 동일 형태를 갖는 데이터의 모임, 하나의 이름
    - 자바스크립트, 파이썬의 배열은 리스트
    - array와 list 구분은 확장/축소 가능 여부로 판단
    - 배열 내 데이터는 비교 가능해야함
    - 생성
        - let 이름=[초기 값 나열]
        - let 이름=new Array(값 나열)
        - let 이름=new Array(개수) → 공간만 배정
    - 접근법
        - 이름[인덱스]
        - ex) let names=[”A”,”B”]  
            names[0] → ”A”  
            names[1] → “B”
            
    - 배열 데이터 개수 확인 : 배열.length

## 데이터 종류 변환 ##
- Number(문자열)
- String(숫자) → 출력
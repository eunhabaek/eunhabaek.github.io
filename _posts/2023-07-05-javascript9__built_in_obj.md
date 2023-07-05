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
- 프로그래밍에 필요한 built-in 객체 

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
  
#### 4. 배열 관련 함수 가진 객체
  - length: 배열 데이터 개수 파악
  - for in: 배열 순회
  - forEach:
    - 배열.forEach((element, index, array)=>{  
        실행 내용})  
    - 배열 순회하며 데이터 하나씩 접근
  - map: 데이터 변환
    - 사용 형태는 forEach와 동일하나, 함수가 데이터를 리턴해야 함
    - 리턴한 데이터 모아서 다시 배열로 리턴
    - Map-Reduce
      - 분산 데이터 처리 후 결과 모음
      - 병렬 처리 가능
  - filter: 추출
    - boolean 검사 후 추출
  - sort: 정렬
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

## BOM: (browser object model): 브라우저 관련 객체
- window나 navigator 객체
- window 안에는 내장 함수, timer
- navigator 객체의 userAgent 속성에 접속한 운영체제와 브라우저 확인 가능한 문자열

## DOM (document object model): body 태그 관련
- body에 만든 요소 찾기
- document.getElementByID(아이디명)
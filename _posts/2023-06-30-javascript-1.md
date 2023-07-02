---
title: "javascript (1)"
excerpt: "javascript의 기초"

categories:
  - frontend
tags:
  - javascript
last_modified_at: 2023-06-30
--- 
## **Javascript란** ##
- HTML 동적 처리 향상을 위한 언어로, HTML에 혼합해서 사용 가능
- 플랫폼(OS, device)에 독립적
- 스크립트 언어 → 웹 브라우저에서 한 줄씩 실행
- 동적 바인딩: 실행 시 메모리 크기 결정, 변경 가능, 느림, 메모리 효율적
- 객체 기반 언어: 클래스 없이 객체 만듦 (객체 지향 X)

## **라이브러리 종류** ##
- jquery -> javascript와 구별 필요
- node.js, express.js → 서버 개발 (jdango, spring과 같은 역할)
- react, vue, angular: spa 개발
- d3js: 시각화
- bootstrap: 반응형 웹디자인 개발
- react-native, ionic: 모바일 앱 개발
- electron: pc 앱 개발

## **Javascript 작성 방법** ##
1. 태그 안에 작성 (인라인 코드)
    - \<태그 이벤트이름=”코드\>
    ```html
    <body>
    <!--인라인 코드-->
        <button onclick="alert('버튼 클릭')">버튼</button>
    </body>
    ```
    
2. html 문서에 영역 삽입
    - \<script\>내용\<script/\>
    ```html
    <body>
        <button id="btn">버튼</button>
        <script>
            document.getElementById("btn").addEventListener("click",
            function(){
                alert("CLICK!!");alert("연속 호출")
            })
        </script>
    </body>
    ```

3. 별도 js 파일 불러오기
    - \<script src=”.js 파일경로”\>\<script/\>

    **외부 .js 파일**
    ```js
        alert("시작하자 마자 수행")
    ```

    **외부 스타일 적용할 본문 html 파일**
    ```html
    <head>
        <script src="./js/basic.js"></script>
    </head>
    ```
    

## **주석 작성 방법** ##
1. 한 줄 주석
    - //주석내용
2. 블럭 주석
    - /* …주석내용….*/
3. 해석 안하고 문자열 처리→ 예약어 처리 가능
    - <![CDATA[ 내용 ]]>


## **메세지 출력** ##
1. 대화 상자로 출력
    - alert()

    ```html
    <body>
        <script>
            alert("대화상자에 출력")
        </script>
    </body>
    ```

2. HTML 문서내에 출력
    1. document.write(): 모아서 출력
    ```html
    <body>
        <script>
            document.write("화면에 출력")
        </script>
    </body>
    ```
    2. document.writeln(): 바로 출력

3. 검사창의 console 출력
    - console.log()
    ```html
    <body>
        <script>
            console.log("콘솔에 출력")
        </script>
    </body>
    ```



- 규칙
    - 대소문자 구분
    - 줄단위 실행
    - 한 줄에 2개 이상 명령어 실행할경우 ; 로 구분 (=나 함수사용)
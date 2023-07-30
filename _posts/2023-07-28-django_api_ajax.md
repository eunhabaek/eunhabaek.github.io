---
title: "Django Rest API에서 Ajax 활용 2"
excerpt: "Django REST API에서 form을 이용한 Ajax 요청 처리하기"

categories:
  - database
tags:
  - Django
last_modified_at: 2023-07-28
---


## Ajax

> **비동기적으로 서버 데이터 요청**하는 것. **다른 프로그램은 영향 주지 않고** UI를 실시간으로 갱신하는 것이 목적
> 
- 최근 웹서버와 웹클라이언트를 별개의 프로그램으로 구현 → 웹 클라이언트가 웹서버에 요청하는 과정 필요함
- **작업과정: 요청(파라미터 전송)→ 응답→응답데이터 파싱(txt, xml, json)→ 사용**

## Form 이용 데이터 삽입

- ajax.html 수정- body부분에 form 작성
    
    ```html
    <body>
    <div id="display"></div>
    <!-- ajax 요청의 경우는 id만 고려, submit 버튼은 모두 고려 -->
    <!-- axtion은 처리할 서버 url, method는 전송 방식 (default=get) -->
    <!-- enctype은 파일이 있는 경우  multipart-formdata 설정 
    => 잘려서 전송된 파일 다르게 도착 합칠 때 필요함 -->
    
    <form id="formid"> <!--action="", method="", enctype="",-->
        <p>이름<input type="text" name="name"></p>
        <p>나이<input type="number" name="age"></p>
        <p>도시<input type="text" name="city"></p>
        <p>연락처<input type="text" name="tel"></p>
        <p>생년월일<input type="date" name="birthd"></p>
        <p><input type="button" value="입력" id="insert"/></p>
    </form>
    </body>
    ```
    
- ajax.html script 부분에 ajax 요청 받아서 처리하는 부분 작성
    
    ```jsx
    <script>
    document.getElementById("insert").addEventListener("click",
            (e) => {
                //form 데이터 작성
                let formid=document.getElementById("formid");
                let formdata=new FormData(formid);
    
                //요청 객체
                let req=new XMLHttpRequest();
    
                //요청 생성
                req.open('POST','../example/post/',true);
    
                //formdata POST 방식 데이터로 변경
                req.setRequestHeader("Content-type",
                "application/x-www-form-urlencoded");
    
                let param="";
                for (let pair of formdata.entries()){
                    param+=pair[0]+"="+pair[1]+"&";
                }
    
                //요청 전송
                req.send(param);
    
                //응답 처리
                req.addEventListener("load",(e) => {
                    alert(req.responseText);
                })
            })
    </script>
    ```
    
- 서버 접속(http://localhost:8000/ajax/)하여 입력 form에 데이터 입력
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d8a855f4-ca14-409e-bd9e-6513fe06c474/Untitled.png)
    
- DBeaver에서 데이터 전송 되었는 지 확인
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6fcbb345-c99c-4542-9c3d-10b3ae0d3ed8/Untitled.png)

### XMLHttpRequest

- call back(이벤트 발생 시 별도로 처리하는 함수)
    
    ```jsx
    //요청 전송
    req.send(param);
    
    //응답 처리
    req.addEventListener("load",(e) => {
    alert(req.responseText);
    ```
    
- call back 함수의 단점
    - call back 함수가 중첩될 경우 코드의 가독성이 떨어짐

## Fetch API

- ajax.html body 부분에 클릭 객체 생성
    
    ```html
    <body>
    	<h3>Fetch API</h3>
    	<button id="fetchbtn">Fetch API</button>
    </body>
    ```
    
- ajax.html script 부분에 fetch api 코드 작성 ⇒ ajax보다 간결
    
    ```jsx
    <script>
    		document.getElementById("fetchbtn").addEventListener('click',
            (e)=>{
            //fetch 함수가 서버에게 요청을 전송
    				fetch('../example/post/'/*,{전송방식, 파라미터 옵션 설정}*/)
    						//then에 함수 설정 시 then에게 전송받은 데이터가 전달 됨
                //json 데이터일 경우 json 메서드 이용해서 파싱 후 다음 함수에 전달
                .then((response)=>response.json())
                .then((data)=>alert(data))
    	          .catch((error)=>alert(error)); //에러 발생 시 예외처리
            });
    </script>
    ```
    
- 서버 () 접속하여 데이터 확인

## Axios library

> ajax 편리하게 사용할 수 있도록 해주는 javascript 라이브러리
> 

**💡 tutorial page: https://axios-http.com/kr/docs/intro**

⇒ 한국어로 자세한 설명 나와있음
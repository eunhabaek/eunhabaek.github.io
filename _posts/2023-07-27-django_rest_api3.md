---
title: "Django Rest API에서 Ajax 활용"
excerpt: "Django REST API에서 Ajax 요청 처리하기"

categories:
  - database
tags:
  - Django
last_modified_at: 2023-07-27
---
## Web client에서 서버로 데이터 요청

1. **ajax**(비동기적)
    - 한 번 요청하고 응답 오면 연결 해제
    - header가 web socket 보다 큼
2. **web socket**(tcp 연결)
    - 연결 후 close 호출하지 않으면 연결 유지
    - 짧은 메세지 자주 전송하면 ajax보다 효율적
3. **SSE**(server sent event= web push)
    - 서버에게 구독 신청을 하면 서버가 데이터를 일방적으로 보내주는 방식
    - 한 번 구독 신청을 하면 요청하지 않아도 데이터 받을 수 있음
    - 알림 받는 방식
4. **모바일 푸시**
    - 스마트폰 앱에서 알림을 받는데 이용
    - APNS, FCM
    

## Ajax 속성

- 구현 방법
    - XMLHttpRequest, Promise, Fetch API, **Axios** 이용
- 객체 생성
    
    new XMLHttpRequest()
    
- 객체 속성
    - **responseText**: XML 이외의 데이터
    - **responseXML**: XML 데이터
- 객체 메서드
    - **setRequestHeader**(헤더이름, 데이터): 헤더 설정
    - **open**( 전송 방식, 전송할 URL, 비동기 전송 여부): 연결 설정
    - **send**(데이터): 요청 전송
    - **sendAsBinary**(데이터): 바이너리 데이터(파일) 전송
- 이벤트
    - **load**: 데이터 전부 받으면 호출되는 이벤트
    - **error:** 데이터 전송 받는 중 에러

## Ajax 실습 - 데이터 조회하기 (GET 방식)

### 1. 모든 데이터 가져오기

- 프로젝트의 urls.py
    
    ```python
    from django.contrib import admin
    from django.urls import path, include
    from apiserverapplication import views
    
    urlpatterns = [
        #ajax 요청 시 처리
        path('ajax/',views.ajax)
    
    ]
    ```
    
- [views.py](http://views.py) 파일에 ajax 함수 생성
    
    ```python
    from django.shortcuts import render
    
    #ajax 처리시 출력할 html 파일 연결
    def ajax(request):
        return render(request,"ajax.html")
    ```
    
- app 디렉토리에 templates 디렉토리 생성한 후 하위에 ajax.html 파일 생성
    
    ```python
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>AJAX</title>
    </head>
    <body>
    <div id="display"></div>
    <button id="allbtn">All Data</button>
    </body>
    
    <script>
        document.getElementById("allbtn").addEventListener("click",
            (e)=>{
            let req= new XMLHttpRequest();
            //출력 함수에 객체 대입 시 toString 메서드 호출
                //python은 __str__ 호출
                //위 메서드 재정의 안하면 기본 메서드 호출
    
                //요청 생성
                //현재 경로는 localhost
                //req.open('전송방식','url',비동기여부);
                req.open('GET','../example/post',true);
    
                //요청 전송
                req.send('')//파라미터 없으므로 공백
    
                //응답 오면 호출
                req.addEventListener('load',(e)=>{
                    //문자열 출력
                    //alert(req.responseText)
    
                    //자바스크립트 데이터 (객체 배열) 출력
                    let data=JSON.parse(req.responseText)
    
                    //출력 내용
                    let txt='';
                    for(let item of data){
                        txt+='<h2>'+'Name: '+item.name+'</h2>';
                        txt+='<h3>'+'Age: '+item.age+'</h3>';
                        txt+='<p>'+'Phone Number: '+item.tel+'</p>';
    
                    }
                    document.getElementById('display').innerHTML=txt;
                })
    
            })
    
    </script>
    </html>
    ```
    
- 출력 되는지 확인
    
    ![Untitled](/figures/django_api6.png)
    

### 2.입력 받아서 name과 일치하는 1개 데이터만 가져오기

- app 디렉토리/ templates 디렉토리에 ajax.html 파일 수정
    
    ```python
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>AJAX</title>
    </head>
    <body>
    <div id="display"></div>
    <p>입력창</p>
    <br/>
    <input type="text" id="name"/>
    <button id="onebtn">One Data</button>
    
    </body>
    <script>
        document.getElementById("onebtn").addEventListener("click",
            (e)=>{
                let name=document.getElementById('name').value;
                //alert(name)
    
                let req= new XMLHttpRequest();
                req.open('GET','../example/one_post/'+name+'/',true);
                req.send('');
                req.addEventListener('load',(e)=>
                {
                   // alert(req.responseText)
                    let data=JSON.parse(req.responseText);
                    let txt=" ";
                    if ('detail' in data){
                        //데이터 없을 때 처리
                        txt="<h3>해당되는 name이 없습니다!</h3>"
                    }else{
                        txt+="<h3>"+"Name: "+data.name+"</h3>";
                        txt+="<h3>"+"Age: "+data.age+"</h3>";
                        txt+="<h3>"+"Tel: "+data.tel+"</h3>";
                    }
                    document.getElementById('display').innerHTML=txt;
                    })
            });
    </script>
    ```
    
- 출력되는지 확인
    
    ![Untitled](/figures/django_api7.png)
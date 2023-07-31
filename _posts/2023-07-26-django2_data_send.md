---
title: "Django에서 GET 전송"
excerpt: "Django GET 요청 처리"

categories:
  - Webserver
tags:
  - Django
last_modified_at: 2023-07-26
---

## admin 기능 활성화

- 관리자 테이블 생성
    
    ```bash
    python manage.py migrate
    ```
    
- 관리자 생성
    
    ```bash
    python manage.py createsuperuser
    ```
    
    - user ID, email, password(8자 이상) 입력
- 관리자 생성 여부 확인
    - 서버 실행
    - http://127.0.0.1/admin/ 접속 후 로그인
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4c492846-8de1-4e87-b2ac-4095099ee96e/Untitled.png)
        

## 요청 처리하기

### html로 전송 방법

1. html만 전송
    - 많은 내용을 출력하기 어려움
    - 전송방법
        - url 등록 : urls.py
            
            ```bash
            from django.contrib import admin
            from django.urls import path
            
            #앱 import
            from testapp import views
            
            urlpatterns = [
                path("admin/", admin.site.urls),
                #첫번째 매개변수는 내용 없으면 localhost:8000 처리
                #두번째 매개변수는 요청에 사용할 views 파일의 함수명
                path("",views.index)
            ]
            ```
            
        - [views.py](http://views.py) 파일에 [urls.py](http://urls.py)에 등록할 함수 생성
            
            ```bash
            from django.shortcuts import render
            from django.http import HttpResponse
            
            #요청 처리하는 함수의 매개변수는 request
            #request 객체 안에는 클라이언트 정보 포함
            
            #index 함수 작성
            def index(request):
                print(dir(request)) #콘솔에서 클라이언트 정보 확인 가능
                return HttpResponse("<p>안녕하세요</p>")
            ```
            
        - 웹서버 접속하여 확인
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4000f91b-0d4b-4f5f-b7d3-2f4d4a44457b/Untitled.png)
            
    - 전송방법1
        - urls.py에 처리할 함수 설정
            
            ```bash
            from django.contrib import admin
            from django.urls import path
            
            #앱 import
            from testapp import views
            
            urlpatterns = [
                path("admin/", admin.site.urls),
            
                #disp라는 요청 처리
                #views파일의 disp함수 실행 
                path("disp",views.disp)   
            ]
            ```
            
        - views.py에 요청 처리할 함수 생성
            
            ```bash
            from django.shortcuts import render
            from django.http import HttpResponse
            
            #외부 html 불러오는 disp 함수 생성
            def disp(request):
                return render(request,"disp.html")
            ```
            
        - 앱 폴더 내의 templates 디렉토리 만들어서 출력할 html 파일 생성
            
            ```html
            <!DOCTYPE html>
            <html lang="en">
            <head>
                <meta charset="UTF-8">
                <title>Title</title>
            </head>
            <body>
            <p>안녕하세요</p>
            <br/>
            <p>Hi</p>
            <br/>
            <p>Hello</p>
            
            </body>
            </html>
            ```
            
        - 데이터 전송 여부 확인하기([http://localhost:8000/disp/](http://localhost:8000/disp))
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/37170dc7-e050-4e60-87db-257d91808a56/Untitled.png)
            
2. **Template Engine** 이용
    - server side rendering
    - 서버에서 생성한 데이터와 함께 html 만들어 전송
    - 기본전송방법
        - urls.py에 처리할 함수 설정
            
            ```bash
            from django.contrib import admin
            from django.urls import path
            
            #앱 import
            from testapp import views
            
            urlpatterns = [
                path("temp",views.temp)
            
            ]
            ```
            
        - views.py에 요청 처리할 함수 생성
            
            ```bash
            from django.shortcuts import render
            from django.http import HttpResponse
            
            #temp 함수 생성
            def temp(request):
                message="안녕하세요!"
                info={"name":"eunha","age":27}
                animals=["cat","dog","rabbit","monkey"]
            
                #html에 데이터 전송하기 위해서 세번째 매개변수에 dict 만들어서 이름과 데이터 작성
                return render(request,"temp.html",
                              {"message":message,"info":info,"animals":animals})
            ```
            
        - 앱 폴더 내의 templates 디렉토리 만들어서 출력할 html 파일 생성
            
            ```html
            <!DOCTYPE html>
            <html lang="en">
            <head>
                <meta charset="UTF-8">
                <title>Title</title>
            </head>
            <body>
            
            <h1>데이터:{{message|upper}}</h1>
            <h2>이름:{{info.name}}</h2>
            
            <h3>
                {% if info.age >= 30 %}
                    30세 이상
                {% else %}
                    30세 이하
                {% endif %}
            </h3>
            
            <h3>
            <ol>
                {% for temp in animals %}
                    <li>{{temp}}</li>
                {% endfor %}
            </ol>
            </h3>
            
            </body>
            </html>
            ```
            
        - 데이터 전송 여부 확인하기([http://localhost:8000/temp/](http://localhost:8000/disp))
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/86d3b2bb-ea5b-4e6f-991a-9a9ffa2de31f/Untitled.png)
            
    - if 또는 for 사용
        - \{\%if 조건\%\} ~\{\% endif\%\}
            - elif 나 else도 사용가능
                
                ```html
                <h3>
                    {% if info.age >= 30 %}
                        30세 이상
                    {% else %}
                        30세 이하
                    {% endif %}
                </h3>
                ```
                
        - \{\%for 임시변수 in 데이터모임\%\} ~ \{\% endfor \%\}
            
            ```html
            <h3>
                {% for temp in animals %}
                    <li>{{temp}}</li>
                {% endfor %}
            </h3>
            ```
            
    - 날짜 출력
        - \{\{날짜|date: 포맷\}\}
            - 날짜를 포맷에 맞추어 출력
    - 문자열
        - 함수 적용 가능
            - \{\{문자열|문자열메서드\}\}
                
                ```html
                <body>
                <p>안녕하세요</p>
                <br/>
                <p>메세지 출력</p>
                <br/>
                <h1>데이터:{{message|upper}}</h1> 
                <h2>이름:{{info.name}}</h2>
                </body>
                ```
---
title: "Django에서 MySQL 사용하기"
excerpt: "Django와 MySQL 연동"

categories:
  - Webserver
tags:
  - Django
last_modified_at: 2023-07-26
---

## MySQL 사용

- mysqlclient 설치
    
    ```bash
    pip install mysqlclient
    ```
    
- 접속 정보 수정 ⇒settings.py 파일의 DATABASES 수정
    
    ```bash
    DATABASES = {
        "default": {
            "ENGINE": "django.db.backends.mysql",
            "NAME": "dx0717",
            "USER":"root",
            "PASSWORD":"",
            "HOST":"127.0.0.1",
            "PORT":"3306"
        }
    }
    ```
    
- [models.py](http://models.py) 파일에 테이블과 연결될 모델 생성
    
    ```bash
    from django.db import models
    
    # Create your models here.
    class ItemInfo(models.Model):
        id=models.CharField(max_length=50,primary_key=True)
        name=models.CharField(max_length=50)
        price=models.IntegerField()
        info=models.CharField(max_length=50)
        url=models.CharField(max_length=50)
    ```
    
- 모델을 데이터베이스에 반영
    
    ```bash
    python manage.py makemigrations
    python managepy migrate
    ```
    
- 명령 수행 시 관리자를 위한 테이블과 models.py로 만들 클래스를 생성하는데 app명_모델이름
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c44eb773-ad94-42e1-8da8-814694c8965a/Untitled.png)
    
- 만들어진 테이블 구조 확인
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8774aff8-0bb3-4c87-a555-9cf48a2f78ba/Untitled.png)
    

## 데이터 조회

### 조회 방법

- model class명.objects.조회메서드( ) 형태로 조회
    
    ```bash
    iteminfo.objects().all()
    ```
    

### 조회 메서드

- **all( )**
    - 테이블의 모든 데이터를 조회
- **get**( 기본키=값)
    - 1개만 조회
- **filter**( 필드이름 연산자 값)
    - 조건에 맞는 데이터 조회
- **exclude**( 필드이름 연산자 값)
    - 조건에 맞지 않는 데이터만 조회
- **count**( )
    - 데이터 개수
- **order_by**( )
    - 정렬할 필드 이름을 나열, 필드명 앞에 - 붙이면 내림차순
- **distinct**( 필드명)
    - 필드 중복제거
- **first**( ), **last**( )
    - 첫번째, 마지막 데이터 조회

## 샘플 데이터 삽입

- DBeaver에서 샘플데이터 삽입
    
    ```sql
    insert into testapp_iteminfo values("1","수박",7000,"할인중","watermelon.jpg");
    insert into testapp_iteminfo values("2","메론",12000,"할인없음","melon.jpg");
    insert into testapp_iteminfo values("3","사과",9000,"10%할인","apple.jpg");
    insert into testapp_iteminfo values("4","복숭아",18000,"5%할인","peach.jpg");
    insert into testapp_iteminfo values("5","오렌지",15000,"할인없음","orange.jpg");
    ```
    
- urls.py에 전체 데이터 가져오기 요청 처리 할 코드 추가
    
    ```python
    from django.contrib import admin
    from django.urls import path
    
    from testapp import views
    
    urlpatterns = [
        path("",views.mysqlimport)
    ]
    ```
    
- [views.py](http://views.py)에 요청 처리할 메소드 생성
    
    ```python
    from testapp from models
    
    from django.shortcuts import render
    from django.http import HttpResponse
    
    #mysql에서 데이터 가져오는 메서드
    def mysqlimport(request):
    		#all() 전체 데이터 조회
        data=ItemInfo.objects.all()
        
    		#filter로 조건 설정 가능 비교연산 불가?
        #data=ItemInfo.objects.filter(price = "18000")
    
    		return render(request,'mysqlimport.html',{"data":data})
    ```
    
- 출력 위한 html 파일 생성
    
    ```html
    !DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
    <h2>
        {% for item in data %}
            {{item.id}}
            <br/>
            {{item.name}}
            <br/>
            {{item.price}}
            <br/>
            {{item.info}}
            <br/>
            <br/>
        {% endfor %}
    </h2>
    </body>
    </html>
    ```
    

## Web client→Web server 요청방법

1. a 태그의 href 속성에 url을 작성한 후 클릭
    - 클릭해서 링크 이동하는 방식
2. form을 만들고 submit 수행
    - 입력 받아 검색 등 수행, 로그인
3. ajax 이용
    - 비동기적 요청, 다른작업 수행하면서 동시에 독립적으로 수행되는 작업
4. web socket 이용
    - 계속 연결해두고 주고받는 방식
5. 브라우저에 url 직접 입력
    - 메인페이지 접속 외에는 사용하지 않음

## HTTP/HTTPS  요청처리

- client가 서버와 연결 가능한 지 확인하고 요청을 전송
- 요청 처리후에 연결 해제

## Parameter

> 웹 클라이언트가 웹 서버에게 전송하는 데이터
> 

### GET 전송

- URL에 파라미터 붙여서 전송
- 장점
    - 자동 재전송 가능
    - 속도가 빠름
- 단점
    - 데이터 크기에 제약이 있음
    - 보안이 취약
- 조회 등 일반적으로 데이터 크기가 작고 보안이 중요하지 않은 요청에 사용
- 데이터에 숫자나 영문이 아닌 것이 있으면 encoding 필요

### POST 전송

- 파라미터를 header, body 등에 숨겨서 전송
- 장점
    - 보안이 우수
    - 데이터 크기에 제약이 없음
    - encoding 필요 없음
- 단점
    - get에 비해 속도가 느림
- 삽입, 삭제, 갱신 등에 사용하며, 보안이 필요한 조회에도 사용
- 최근 삽입 POST, 수정 PUT, 삭제 DELETE ⇒ for ai /robot

## Parameter  작성

### GET

1. URL 뒤에 파라미터를 이름과 함께 작성
    - 요청경로?파라미터이름=파라미터 값&파라미터이름=…
    - 파라미터가 두개 이상일 때 사용
    - form 데이터 전송에 이용
    - ex) https://news.naver.com/main/main.naver**?mode=LSD&mid=shm&sid1=101**
2. URL에 파라미터를 포함시키는 경우
    - 요청경로/파라미터값
    - 파라미터가 한개일 때
    - 상세보기 구현에 이용
    - ex) https://ggangpae1.tistory.com**/672**

### POST

1. formdata 로 전송
2. JSON 형식으로 전송

## Django 서버에서 Parameter 읽기

- 전송 방식에 따라 다르게 읽음

### url에 포함시켜서 parameter 전송 예시

1. html 파라미터 요청 경로 만들기
    
    ```python
    <h2>
        {% for item in data %}
            {{item.id}} - <a href="detail/{{item.id}}"> {{item.name}}</a> - {{item.price}} - {{item.info}}
            <br/>
            <br/>
        {% endfor %}
    </h2>
    ```
    
2. [urls.py](http://urls.py) 
    - path(’요청경로/<파라미터자료형:파라미터명>’,처리할 함수나 클래스)
        
        ```python
        path("detail/<str:id>",views.detail)
        ```
        
3. views.py
    - def 함수명(request, 파라미터명):
        
        ```python
        def detail(request,id):
            item=ItemInfo.objects.get(id=id)
            return render(request,'detail.html',{'item':item})
        ```
        
4. 함수 적용할 html 생성
    
    ```python
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
    <h3>
      이름:{{item.name}}<br/>
      가격:{{item.price}}<br/>
      할인율:{{item.info}}<br/>
    </h3>
    </body>
    </html>
    ```
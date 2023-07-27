---
title: "Django Rest API 실습하기"
excerpt: "Django로 MySQL 연결하여 REST API 데이터 불러오기"

categories:
  - database
tags:
  - Django
last_modified_at: 2023-07-27
---

## 샘플 URL 처리

1. 프로젝트의 urls.py에서 url 처리할 함수와 클래스를 연결
    - 프로젝트 urls.py에 직접 연결 시키면 app이 여러개일때 코드가 너무 길어지고 가독성 떨어짐
    - 각 app이 url을 처리하도록 설정 수정
        
        ```python
        from django.contrib import admin
        from django.urls import path, include
        
        urlpatterns = [
            path("admin/", admin.site.urls),
        		
        		#라우팅 작업
            #example로 시작하는 url은 apiserverapplication.urls.py에서 처리하도록 설정
            path('example/',include('apiserverapplication.urls'))
        ]
        ```
        
2. application 폴더에 [urls.py](http://urls.py) 추가하고 URL과 처리할 함수 및 클래스 연결
    
    ```python
    from django.urls import path
    from .views import helloAPI
    
    urlpatterns=[
        #example/hello 요청이 오면 helloAPI 함수가 처리
        path("hello/",helloAPI)
    ]
    ```
    
3. [views.py](http://views.py) 파일에 helloAPI 메서드 작성
    
    ```python
    from django.shortcuts import render
    
    from rest_framework.response import Response
    from rest_framework.decorators import api_view
    
    #GET 요청만 처리 -조회 작업
    @api_view(['GET'])
    def helloAPI(request):
        return Response("Hello Rest API!!s")
    ```
    
4. 웹브라우저에서 확인
    - 서버 실행 후 [localhost:8000/example/hello](http://localhost:8000/example/hello) 경로 접속
        
        ![Untitled](/figures/django_api.png)
        
5. Resonse 클래스
    - 응답 결과를 위한 클래스
    - 첫번째 매개변수는 데이터, status는 상태코드
        - status 없으면 어떤 이유로 실패했는 지 알 수 없음
    - 상태코드
        - **200**: 정상
        - **300**: 리다이랙션
        - **400**: 클라이언트 오류
            - **403**: forbidden 권한부족
            - **404**: url 오류, 처리 불가
        - **500**: 서버 에러

## 데이터 삽입 처리 - POST 방식

- 처리할 함수를 생성- views.py
    
    ```python
    from rest_framework.response import Response
    from rest_framework.decorators import api_view
    from rest_framework import status
    from .serializers import InforSerializer
    
    #GET 요청만 처리
    @api_view(['GET'])
    def helloAPI(request):
        return Response("Hello Rest API!!")
    
    @api_view(['POST']) #메소드
    def Inforpost(request):
    
        print("1") #실행 안될 시 메소드와 url 연결 실패
        #클라이언트 데이터를 모델이 사용 가능한 데이터로 변환
    
        serializer=InforSerializer(data=request.data)
        print("2") #실행 안될 시 serializable 실패
    
        #유효성 검사
        if serializer.is_valid():
    
            print("3") #실행 안될 시 이름 잘못 된 것
            serializer.save() #데이터 저장
    
            #성공 시 데이터 전송
            return Response(serializer.data,
                            status=status.HTTP_201_CREATED)
        #실패시 처리
        Response(serializer.errors,status=status.HTTP_400_BAD_REQUEST)
    ```
    
- url과 함수를 연결- urls.py
    - application urls.py에 작성
        
        ```python
        from django.urls import path
        from .views import Inforpost
        
        urlpatterns=[
            #example/post 요청시 Inforpost 함수가 처리
            path("post/",Inforpost)
        ]
        ```
        
- 서버 접속(http://localhost:8000/example/post/) 후 작동 확인
    - POST에 JSON 형식으로 입력
        
        ![Untitled](/figures/django_api2.png)
        
    - 데이터 입력 정상 완료ㅇ
        
        ![Untitled](/figures/django_api3.png)
        
    - DBeaver에서 데이터 입력 되었는지 확인
        
        ![Untitled](/figures/django_api4.png)
        

## 데이터 1개 가져오기

- 기본키 값을 매개변수로, URL 뒤에 붙여서 전송
- urls.py에 url 처리 함수 등록
    
    ```python
    from django.urls import path
    from .views import Inforpost_one
    
    urlpatterns=[
        #1개 데이터 가져오는 요청
        path("one_post/<int:name>/",Inforpost_one)
    ]
    ```
    
- views.py에 함수 작성
    
    ```python
    from rest_framework.response import Response
    from rest_framework.decorators import api_view
    from rest_framework import status
    from .serializers import InforSerializer
    from .models import Infor
    
    #기본키 가지고 데이터 못찾아오면 404 error
    from rest_framework.generics import get_object_or_404
    
    @api_view(['GET'])
    def Inforpost_one(request,name):
        #기본키로 데이터 1개 가져오기
        info=get_object_or_404(Infor,name=name)
        serializer=InforSerializer(info)
        return Response(serializer.data,status=status.HTTP_200_OK)
    ```
    
- [http://localhost:8000/example/one_post/은하/](http://localhost:8000/example/one_post/%EC%9D%80%ED%95%98/)  접속하여 데이터 조회 확인
    - 기본키 name이 “은하”인 데이터가 1개 조회됨
        
        ![Untitled](/figures/django_api5.png)
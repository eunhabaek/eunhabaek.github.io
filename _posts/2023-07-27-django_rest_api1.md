---
title: "Django Rest API 시작하기"
excerpt: "Django로 MySQL 연결하여 REST API 만들기"

categories:
  - database
tags:
  - Django
last_modified_at: 2023-07-27
---

## API(Application program interface)

> **프로그램과 프로그램을 연결시켜주는 매개체**
> 
- 데이터의 형태로 제공될 수도 있고 함수, 클래스 , 패키지, 프레임워크 형태로도 제공
- **Open API**
    - 누구나 사용 가능하도록 공개한 것
- API server 구축 시 **restful**하게 구성

## **CSI** (client side rendering)

- API server는 화면 구성하지 않고 데이터만 제공
- 클라이언트 프로그램에서 데이터 받아서 화면 출력

## REST

> **분산 하이퍼미디어 시스템을 위한 소프트웨어 아키텍쳐**
> 

### **REST의 원칙**

1. **인터페이스의 일관성**
    - 어떤 디바이스든 동일한 작업은 동일한 url 사용
2. **무상태(stateless)**
    - 클라이언트의 컨텍스트 서버에 저장 X
3. **캐싱기능 활용**
4. **계층화**
    - Client ↔ 연결서버 ↔ Middle ware(보안, Load balancing) ↔ API server(작업 및 결과 생성) ↔ Data server
    - 재사용성, 작업 안정성
5. **Code on Demand**
    - 기능 확장 가능하도록 생성
6. **클라이언트와 서버 분리**

## Django REST API 프로젝트 생성

### 1. 프로젝트 디렉토리 생성

### 2.  필요한 패키지 설치

- django, mysqlclient djangorestframework

### 3. django 프로젝트 생성

- apiserverproject

### 4. Django application 생성

- apiserverapplication

### 5. [settings.py](http://settings.py) 수정

- INSTALLED_APPS 수정
    
    ```python
    INSTALLED_APPS = [
        "django.contrib.admin",
        "django.contrib.auth",
        "django.contrib.contenttypes",
        "django.contrib.sessions",
        "django.contrib.messages",
        "django.contrib.staticfiles",
        "apiserverapplication",
        "rest_framework"
    ]
    ```
    
- DATABASES 수정
    
    ```python
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
    
- TIME_ZONE 수정
    
    ```python
    TIME_ZONE = "Asia/Seoul"
    ```
    

### 5. 프로젝트에 사용한 테이블 생성 및 데이터베이스 연결 확인

1. [models.py](http://models.py) 파일에 필요한 모델 작성
    
    ```python
    from django.db import models
    
    class Infor(models.Model):
        name=models.CharField(max_length=50,primary_key=True)
        age = models.IntegerField()
        city = models.CharField(max_length=50)
        tel = models.CharField(max_length=50)
        birthd = models.DateField()
    ```
    
2. [models.py](http://models.py) 변경내용 데이터베이스에 적용
    
    ```python
    python manage.py makemigrations
    python manage.py migrate
    ```
    
3. 변경내용 적용 확인
    
    ```python
    use DB명;
    show tables;
    desc 테이블명(app이름_모델명)
    ```
    

### 6. 직렬화(serializable)

> 프로그램 객체 전송 위한 작업
> 
- client 에게 받은 데이터를 python 객체로 변환 및 python 객체를 client에게 전송하기 위해 JSON 문자열로 변환하는 역할
- 이 작업 수행 시 Response 리턴, 미수행시 JsonResponse 리턴
- 애플리케이션에 [serializers.py](http://serializers.py) 파일을 만들고 작성
    
    ```python
    from rest_framework import serializers
    from .models import Infor
    
    class InforSerializer(serializers.ModelSerializer):
        class Meta:
            model=Infor
            fields=["name","age","city","tel","birthd"]
    ```

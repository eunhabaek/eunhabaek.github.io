---
title: "Django 시작하기"
excerpt: "Django 설치 및 앱 실행"

categories:
  - Webserver
tags:
  - Django
last_modified_at: 2023-07-26
---

## 가상환경

> 프로그램을 독립적으로 실행시키는 환경
> 

- 라이브러리를 별도로 설치하기 위해 만든 환경
- 서비스를 만들 때는 반드시 가상 환경을 생성해야 함
- c나 python은 설치 없어도 실행 가능 (소스 코드와 라이브러리 합쳐서 별도 실행 프로그램 생성)→ 실행프로그램 만들 때 라이브러리 포함시켜야 함
- 자바는 jdk(자바 제공 api) 포함하지 않고 실행 프로그램 생성파므로 자바는 jdk가 제공하는 라이브러리가 존재해야 함(jre)
- pycharm은 기본적으로 가상환경 생성해줌

## Deamon

> 실행중인 프로그램 계속 동작시키는 것
> 

## Django

### 1. 가상환경 생성

- 터미널에서 작업
    
    ```bash
    #가상환경 생성
    python3 -m venv 가상환경이름
    
    #가상환경 활성화
    activate
    ```
    

### 2. Django 설치

- 터미널 작업
- 가상환경에서는 프로젝트 만들 때마다 계속 설치해주어야 함
    
    ```bash
    pip install django
    ```
    

### 3. Django project 생성

- 터미널 작업
    
    ```bash
    django-admin startproject project1 .
    ```
    
- 프로젝트 디렉토리와 manage.py 생성
- mange.py는 실행 파일, main 함수를 포함
- python의 기본 entry 파일 명은 main.py

### 4. Django application 생성

- 터미널 작업
    
    ```bash
    python manage.py startapp testapp
    ```
    

### 5. application 실행

- python manage.py runserver 외부접속 IP :포트번호
- IP와 포트 생략 시 로컬로만 접속 가능
    
    ```bash
    python manage.py runserver
    ```
    
- 도커가 메인 페이지 제공(127.0.0.1) ⇒ loopback
    - localhost는 내부 접속
    - loopback은 외부 나갔다가 다시 접속 → 방화벽의 영향 받음
- http://127.0.0.1:8000 접속
    
    ![Untitled](/figures/django_api.png)
    

## Django의 project 및 app 파일 setting

### 1. [settings.py](http://settings.py)

- **ALLOWED_HOSTS**
    - 접속 가능 ip설정
    - 기본적으로는 로컬에만 접속 가능
- **TIME_ZONE**
    - 시간 대역 설정
    - 한국시는 “Asia/Seoul”
- **DATABASES**
    - 접속 가능한 DB 설정
    - 기본 DB는 sqlite3
- **INSTALLED_APPS**
    - application 등록
    - python [manage.py](http://manage.py) startapp 명령어로 생성한 앱 이름 등록

### 2. urls.py

- 클라이언트의 요청 URL을 처리할 함수나 메서드 등록 파일
- 서버는 여기서 등록된 요청만 처리 가능

### 3. models.py

- RDBMS와 연동할 모델을 등록
- 2개 수행 단계
    - python [manage.py](http://manage.py) make migrations
    - python [manage.py](http://manage.py) migrate ⇒ 테이블 생성
- migration 수행되지 않으면 migrations 디렉토리의 __init__.py를 삭제하고 다시 수행

### 4. views.py

- 클라이언트 요청 로직 작성, 호출
- 함수 또는 클래스로 작성 가능
- 대부분의 django 샘플 코드에서는 직접 작성하나, 실무에서는 별도 파일을 호출하는 형식
    - 직접 작성하는 이유는 간단한 로직일 때
- django의 view는 다른 프레임워크의 controller의 역할을 수행
    - controller는 클라이언트의 요청을 받아서 필요한 로직을 호출하여 결과를 화면에 출력하거나 클라이언트에게 전송
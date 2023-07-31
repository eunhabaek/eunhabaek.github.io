---
title: "Web Programming"
excerpt: "Web server 종류와 프레임워크"

categories:
  - Webserver
tags:
  - Django
last_modified_at: 2023-07-25
---

## Web Service 구현 방식

### 1) CGI(Common Gateway Interface) 와 Application Server

- CGI
    - 클라이언트의 요청을 프로세스를 이용해서 처리하는 방식
    - Perl 이나 C로 만든 프로그램을 직접 실행
- Application Server
    - 클라이언트의 요청을 스레드를 이용해서 처리하는 방식
    - Java 나 Python을 이용해서 클라이언트을 요청을 처리

### 2) Server Rendering 과 Client Rendering

- Server Rendering
    - 서버가 클라이언트의 출력 내용을 만들어서 HTML로 변환해서 전송하는 방식
    - Java 의 JSP 출력이나 Template Engine을 사용하는 방식
    - Python 의 Flask 나 Django를 이용한 방식
- Client Rendering
    - 서버가 클라이언트에게 데이터를 전송해서 클라이언트가 직접 화면을 만들어가는 방식
    - Server는 출력 내용을 만들지 않고 JSON 형태의 데이터만 전송
    - Client 는 ajax 나 websocket 등을 이용해서 데이터 전송받고 출력물을 생성

## Server 상태 코드

> 클라이언트가 서버에게 요청을 하면 서버가 처리 상태를 알려주는 코드
> 

**1XX**: 임시적인 응답으로 현재 클라이언트의 요청까지 처리 됨

**2XX**: 정상 응답

**3XX**: Redirection 중 - 정상 처리한 후 다른 요청으로 이동

**4XX**: 클라이언트 오류

- **401**: 권한 없음

- **403**: 권한 처리 이외의 사유 발생 -> authorization 에러

- **404**: 요청을 처리할 수 없음, URL이 잘못됨

**5XX**: 서버 오류

- **500**: 내부 서버 오류

- **503**: 서비스 불가

## Web Client 와 Server의 통신 방식

### 1) ajax

- 클라이언트에서 비동기적으로 서버에게 요청을 전송하는 방식
- 가장 많이 사용

### 2) web socket

- 클라이언트 와 서버가 연결형 통신을 수행하는 방식
- 직접 close 하지 않는 이상 계속 연결된 상태

### 3) SSE(Server Sent Events - Web Push)

- 서버가 클라이언트의 요청이 없어도 데이터를 전송하는 방식

## Python Web Framework

### 1) Flask 와 Django 의 차이

- 프로젝트 레이아웃
    - Flask 는 하나의 프로젝트에 하나의 애플리케이션을 사용
    - Django는 하나의 프로젝트에 여러 개의 애플리케이션 생성 가능
- RDBMS 사용 여부
    - Flask는 내장된 데이터베이스 활용 라이브러리가 없으므로 별도의 라이브러리를 이용해서 데이터베이스를 사용
    - Django는 ORM을 내장, 별도의 라이브러리 없이 데이터베이스 사용 가능
- Flask는 마이크로 프레임워크라서 편하게 확장할 수 있으며 유연함

### 2) Django를 권장하는 경우

- Web App 이나 API 백엔드의 크기가 큰 경우
- 빠르게 개발해서 배포하고 업데이트 하고자 하는 경우
- CSRF, XSS, SQL Injection, 클릭 재킹 등의 기본적 보안을 쉽게 적용하고자 하는 경우
- 스케일 업, 스케일링 다운(서비스의 개수를 늘리거나 줄이고자 하는 경우)을 자유자재로 하고자 하는 경우
- SQL에 익숙하지 않은 상태에서 RDBMS를 사용하고자 하는 경우

### 3) Flask 사용을 권장하는 경우

- 앱이 작은 경우
- 설계가 가능한 경우
- Python에 익숙하지 않은 경우
- NoSQL을 사용하는 경우
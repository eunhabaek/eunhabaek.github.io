---
title: "AWS CloudFront"

categories:
  - AWS
tags:
  - Network
last_modified_at: 2023-09-08
---

CDN(content delivery network)
-----------------------------

*   리소스 요청 발생 시 사용자가 현대 어디에서 요청하는지에 따라 리소스 전달해주는 분산 네트워크 시스템(distributed network system)

*   목적: 엣지 로케이션을 이용해 사용자에게 최소한의 지연 시간과 최고의 성능으로 콘텐츠 제공

*   많은 요청이 오가는 지역을 근거로 엣지 로케이션 생성

*   엣지 로케이션은 origin의 컨텐츠 복사본 가짐

*   CDN 이용하지 않는 경우
    
    *   웹사이트 호스팅 되는 곳(origin)이 한국인 경우
    
    *   어떤 지역에서 요청하든 무조건 한국에서 콘텐츠 전달
    
    *   동시 다발적 그리고 먼 곳에서 요청 시 지연 시간이 발생

Edge Location
-------------

*   AWS의 CloudFront CDN은 전 세계 곳곳에 Edge location 생성

*   사용자 요청 시 가장 가까운 edgo location에서 데이터 전달받음

*   오리진의 컨텐프 원본을 캐시에 보관하여 속도 빠름

*   앱을 전세계 곳곳에 배포하는 기업들은 대부분 AWS 서비스를 이용함

Origin
------

*   최초 앱 서버 구현되고 돌아가고 있는 곳

*   브라우저의 비동기 요청은 동일 오리진에게만 가능

*   브라우저 비동기 요청으로 다른 오리진에 요청 보낼 시 그쪽 서버에서 CORS(다른 오리진에 요청을 받아들일지) 설정하거나 프록시 서버를 이용해서 요청을 전송하고 응답 받아야 함

AWS CloudFront
--------------

*   CDN을 위한 서비스

*   웹 페이지 호스트와 네트워크 서버 설정을 위한 서비스

AWS CloudFront 배포 생성
--------------------

### 원본 생성

![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/e227e592-61f8-4ba9-bb49-7346dac90f6a)

### 기본 캐시 동작

![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/a19463e3-4915-4380-8b30-e2cd230ffe6e)

*   뷰어 프로토콜 Redirect HTTP to HTTPS 선택

### 웹 애플리케이션 방화벽

![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/66f3a9dc-8da3-4055-8154-a54c486bbc8e)

### 배포 완료
![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/fe28edd5-07e3-4de3-9bab-108fad330a4e)

배포 도메인 접속 확인
------------

![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/869a2554-b643-4802-84af-0a9d4b68b42a)

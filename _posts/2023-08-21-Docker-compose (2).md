---
title: "Docker-compose (2)"

categories:
  - Docker
last_modified_at: 2023-08-21
---

Docker-compose 구조
-----------------

*   주 항목
    
    *   services: 컨테이너
    
    *   networks
    
    *   volumes

*   정의 내용
    
    *   image
    
    *   networks: —net
    
    *   volumes; v
    
    *   ports: -p
    
    *   environments: -e
    
    *   depends\_on: 다른 서비스에 대한 의존 관계
        
        *   다른 서비스를 먼저 실행시킬 때 사용
        
        *   ex) wordexpress, django, spring boot는 DB 먼저 실행되어야 함
            *   depends\_on에 DB 설정 필요
    
    *   restart: 컨테이너 종료시 재시작 여부
        *   no, always, on-failure, unless-stopped

*   버전 정의
    
    *   첫 줄에 명시
    
    *   이 버전에 따라 지원 도커 엔진의 버전이 달라짐
    
    *   3이라고 쓰면 모든 도커 버전을 지원함
    
    *   도커와 쿠버네티스는 업데이터 빠르므로 버전 확인하면서 작성 필요

*   서비스 정의
    
    *   컨테이너 실행하는 것과 동일 개념
    
    *   서비스 이름 정의하고 하위 레벨에 도커 명령과 유사하게 컨테이너 실행에 필요 옵션 작성
        
        *   도커 명령어
            
            *   docker run —name=myweb nginx:latest
            
            *   docker run —name=marizdb marizdb:10.4.6
        
        *   docker-compose 파일
            
                version “3”
                services:
                    mywev:
                        image: nignx:latest
                		mariadb:
                				image: mariadb:10.4.6
            
        
        *   dockefile 이미지인 경우
            
                version “3”
                services:
                    mywev:
                        build .
                		mariadb:
                				build:
                						context:.
                						dockerfile: 도커파일 경로
            

*   하위 옵션
    
    *   container\_name; —name : 컨테이너 이름 설정
    
    *   ports; -p: 포트포워딩 설정
    
    *   expose: 외부로 노출 X, 필요 시 링크로 연결된 서비스 간 통신을 허용 ⇒ 내부 통신 위한 포트
    
    *   networks; —net: 하나의 네트워크로 묶을 때 사용
    
    *   volumes; -v: 볼륨 사용
    
    *   environmnets; -e: 환경 설정
    
    *   command: 서비스가 구동 된 후 실행 할 명령어 작성
    
    *   restart; —restart: 재시작 옵션
    
    *   depends\_on: 서비스간 종속성, 먼저 실행해야 하는 서비스 지정

*   네트워크 정의
    
    *   여러 컨테이너들이 연결 될 수 있도록 해주는 것
    
    *   docker-compose는 네트워크 지정하지 않으면 자체 기본 네트워크를 자동으로 생성
    
    *   최상위 레벨에 networks 지정 시 해당 이름의 네트워크가 생성되고 대역은 172.x.x.x(사설IP)로 자동 할당
        *   사설 아이피 ⇒ 내부에서만 이용 가능

*   볼륨 정의
    
    *   도커 엔진 내부의 공간과컨테이너 사이의 데이터를 공유하기 위한 것
    
    *   컨테이너 내부 데이터 반 영구적 보관 위해 사용

### docker 명령과 docker-compose 비교

*   mysql:8.0과 wordpress:5.7 웹 사용 서비스를 각각의 볼륨 소유하고 하나의 네트워크로 묶기
    
    *   도커 명령어
        
        *   볼륨 만들고 확인
            
            *   docker volume create mydb\_date
            
            *   docker volume create myweb\_data
            
            *   docker volume ls
        
        *   네트워크 만들고 확인
            
            *   docker network create myapp-net
            
            *   docker network ls
        
        *   mysql 컨테이너 생성
            
            *   docker run -dit --name=mysql\_app -v mydb\_data:/var/lib/mysql --net=myapp-net --restart=always -p 3306:3306 -e MYSQL\_ROOT\_PASSWORD=1016 -e MYSQL\_DATABASE=eunha -e MYSQL\_USER=eunha -e MYSQL\_PASSWORD=1016 mysql:8.0
            
            *   docker ps로 확인
        
        *   wordpress 컨테이너 생성
            *   docker run -dit --name=wordpress\_app -v myweb\_data:/var/www/html -v ./myweb-log:/var/log --net=myapp-net --restart=always -p 8001:80 wordpress:5.7 -e WORDPRESS\_DB\_HOST=mysql\_app:3307 -e WORDPRESS\_DB\_NAME=eunha -e WORDPRESS\_DB\_USER=eunha -e WORDPRESS\_DB\_PASSWORD=1016 —link mysql\_app:mysql
    
    *   docker-compose
        
            version: "3"
            services:
              mydb:
                image: mysql:8.0
                container_name: mysql_app1
                volumes:
                  - mydb_data:/var/lib/mysql
                networks:
                  - backend-net
                restart: always
                ports:
                  - "3306:3306"
                environment:
                  MYSQL_ROOT_PASSWORD: 1016
                  MYSQL_DATABASE: eunha
                  MYSQL_USER: eunha
                  MYSQL_PASSWORD: 1016
              myweb:
                image: wordpress:5.7
                container_name: wordpress_app1
                volumes:
                  - myweb_data:/var/www/html
                  - ./myweb-log:/var/log
                networks:
                  - backend-net
                  - frontend-net
                restart: always
                ports:
                  - "8003:80"
                environment:
                  WORDPRESS_DB_HOST: mydb:3306
                  WORDPRESS_DB_USER: eunha
                  WORDPRESS_DB_NAME: eunha
                  WORDPRESS_DB_PASSWORD: 1016   
                depends_on:
                  - mydb
            networks:
              frontend-net: {}
              backend-net: {}
            volumes:
              myweb_data: {}
              mydb_data: {}
        
    
    *   docker-compose 확인
        *   docker-compose ps

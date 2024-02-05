Docker-compose (1)
==================

Docker - Compose
----------------

*   개요
    
    *   시스템 구축 관련 명령을 하나의 텍스트 파일에 기재해 시스템 전체를 한번에 실행, 종류, 폐기 가능하게 도와주는 도구
    
    *   공통 목적 갖는 APP 스택을 docker compose yaml 코드로 작성
    
    *   ex) sDB는 mysql을 사용하고 클라이언트 요청은 node.js로 처리하고 클라이언트 앱은 react로 배포하고자 하는 경우화
        
        *   방법1) 하나의 network로 묶어 배포 ⇒ 권장 X
        
        *   방법2) 별개의 컨테이너로 배포 후 네트워크로 연결하거나 포트포워딩 이용 접근- 별도의 파일 이용 배포
        
        *   방법3) 별개의 컨테이너로 배포 후 네트워크로 연결하거나 포트포워딩 이용 접근- 하나의 파일로 배포 (docker-compose)
    
    *   yaml 포멧 설정 파일으로 여러 container 간의 실행이나 관계 설정 후 일괄 실행 또는 일괄 종료 및 삭제 가능 도구
    
    *   도커 명령어와 유사하나 다른 개념

*   Docker-compose와 Dockerfile 비교
    
    Dokerfile
    
    Docker-compose
    
    \- 하나의 이미지 위함
    
    \- 여러 이미지 모음 (docker run 여러개 모은 것과 같음)
    
    \- 네트워크나 볼륨 생성 불가
    
    \- 네트워크와 볼륨도 한번에 생성 가능
    
    \- kubernetes를 대신 이용하기도 함
    

*   설치
    
    *   mac이나 linux에서 docker desktop 사용시 필요 X
    
    *   windows는 설치 필요

*   예시
    
    *   apache web server(httpd) 만들어 실행
        
        *   단순 명령어
            *   docker run --name apa1 -d -p 8080:80 httpd
        
        *   docker-compose 이용
            
            *   docker-compose.yml 작성
                
                    version: "3"
                    services:
                      apa1:
                        image: httpd
                        ports:
                          - 8081:80
                        restart: always
                
            
            *   docker-compose up 실행
            
            *   docker ps로 새로운 컨테이너 확인
            
            *   localhost:8081로 브라우저 확인 가능
    
    *   mariaBD 컨테이너 생성
        
        *   명령어
            *   docker run --name mariadb -d -p 3306:3306 —restart=always -e MYSQL\_ROOT\_PASSWORD=root mariadb
        
        *   docker compose
            
            *   yaml 파일
                
                    version: "3"
                    services:
                      mariadb:
                        image: mariadb:10.4.6
                        environment:
                    			- MYSQL_ROOT_PASSWORD=root
                    		ports:
                          - 3306:3306
                        restart: always
                
            
            *   docker-compose up 실행
            
            *   docker ps로 새로운 컨테이너 확인
            
            *   dbeaver에서 3306로 브라우저 확인 가능
    
    ### docker-compose 명령
    
    *   build
        *   Dockerfile이용 빌드, 재빌드
    
    *   config
        *   구성 내용 확인
    
    *   create
        *   서비스 생성
    
    *   down
        *   도커 컴포즈 자원 일괄 정지 후 삭제
    
    *   events
        *   컨테이너에서 실시간으로 이벤트 정보를 수신
    
    *   exec
        *   실행중 컨테이너 명령 실행
    
    *   help
        *   도움말
    
    *   images:
        *   사용된 이미지 정보
    
    *   kill
        *   컨테이너 강제 정지
    
    *   logs
        *   실행 로그 정보 출력
    
    *   pause
        *   컨테이너 서비스 일시 정지
    
    *   port
        *   포트 바인딩된 외부로 연결된 포트 확인
    
    *   ps
        *   실행중인 컨테이너 서비스 출력
    
    *   pull
        *   서비스 이미지 가져오기
    
    *   push
        *   서비스 이미지 올리기
    
    *   restart
        *   컨테이너 서비스 재시작
    
    *   rm, run
    
    *   scale
        *   컨테이너 서비스에 대한 컨테이너 수 설정
    
    *   start, stop
    
    *   top
        *   실행 중 프로세스 출력
    
    *   up
        *   컨테이너 서비스 생성과 시작
    
    *   version
        *   버전 확인
    
    ### 컨테이너 서비스 생성 및 시작
    
    *   docker-compose -f 파일경로 up \[옵션\]
        
        *   \-f 옵션은 현재 디렉토리가 아닌 경우만 사용
        
        *   옵션
            
            *   \-d(—detach): 데몬으로 실행(백그라운드)
            
            *   \-build: 컨테이너 실행 전 이미지를 빌드
            
            *   —force-recreate: 설정 또는 이미지가 변경되지 않아도 컨테이너를 재생성
            
            *   \-t; -timeout: 컨테이너 종료 시 타임아웃 설정, 기본 10초
            
            *   —scale 서비스명=개수: 컨테이너 수 변경
            
            *   —abort-on-container-exit: 컨테이너 하나라도 종료 시 모두 종료
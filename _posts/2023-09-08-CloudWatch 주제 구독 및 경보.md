---
title: "Cloud Watch에서 주제-구독 설정 후 경보 보내기"

categories:
  - AWS
last_modified_at: 2023-09-08
---

Cloud Watch
-----------

*   리소스와 앱 실시간 모니터링 서비스

*   로그
    
    *   앱의 리소스 로그를 그룹 단위로 기록
    
    *   CSV로 다운 가능하고 S3에 저장 가능

*   지표
    *   그래프 형태로 출력

*   대시보드
    *   여러 지표를 하나의 화면에 출력

*   종류
    
    *   기본: 5분
    
    *   상세: 1분

*   장점
    
    *   서버 과부하 문제 해결
    
    *   비용 절감 효과 - 사용자에게 알림 제공

실습
--

*   EC2 인스턴스 만들기

*   putty 접속

*   컴퓨터에 용량 큰 파일 다운
    *   [https://www.jetbrains.com/datagrip/download에서](https://www.jetbrains.com/datagrip/download에서) 다운로드

*   EC2 서버에 파일 전송 (local → EC2)
    
    *   pscp -i ppk 경로 전송할 파일 경로 ubuntu@서버IP:전송할경로
    
    *   pscp -i C:\\Users\\USER\\Downloads\\eunhakey.ppk C:\\Users\\USER\\Downloads\\MLOps.pdf [ubuntu@54.198.226.156](mailto:ubuntu@54.198.226.156):

*   putty에서 해당 파일 확인 가능
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/05d8ad49-bf4f-42a0-9ad8-ed0a0bfa63b3)

    

*   인스턴스 모니터링 확인 가능
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/fd351658-8e38-4c3d-8bdf-daa0c39135c7)


*   Cloud Watch에서도 확인 가능
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/aca474a4-216a-4a59-8039-8c73d9ee2c2c)

    

*   파일 업로드 기록은 EBS에서 확인 가능
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/59944a89-c64e-4055-b08a-e435d99d51a7)

    

EC2 인스턴스에서 경고 만들기
=================

*   CPU 사용량이 70%넘는 경우 이메일로 알림 주기

### 1\. 토픽 주제 선정 - 구독 서비스 생성

*   SQS - 구독과 게시

*   SNS - 이메일, 문자 서비스
    
    *   구독만 가능
    
    *   주제 선정은 SNS에서

*   SNS (Simple Notification Service)에서 주제 생성하기
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/96f1cf9b-130d-48c3-ae81-88a3b5065c9d)


*   주제 안에서 구독 생성
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/ac22b974-cb9c-4a3a-8df0-69a83f86a044)

    

*   이메일에서 구독을 confirm
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/f7868e6b-8a39-42d4-822c-860449f04d7b)

    

### 2\. CloudWatch에서 경보 설정

*   scail up/down 등 조절 가능

*   CloudWatch>경보>경보 생성

*   지표 및 조건 지정
    
    *   EC2> 인스턴스별 지표 > 지표 선택> CPUUtilization
        
        ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/87a4657e-013d-4dac-8d7c-aafc73b5ba7a)

        
    
    *   조건 설정
        
        ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/2c95285b-dbec-4c5a-b475-9f1f8c920181)

        

*   작업 구성
    *   알림 설정 (만들어 둔 주제 선택)
        
        ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/ecf55c10-c2cc-416f-9fa2-4f1f2abcdb7f)

        

*   CPU에 stress 가하기
    
    *   stress 설치
        
        *   sudo apt-get update
        
        *   sudo apt-get install stress
    
    *   cpu 확인
        *   grep -c processor /proc/cpuinfo
    
    *   코어 수 1인 경우 스트레스 주기
        *   stress -c 2

*   경보 메일 확인 가능
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/e134c056-d918-45ab-9da5-fee89330fa5c)

---
title: "Oracle 데이터베이스 시작하기"
excerpt: "오라클 설치 및 실행하기"

categories:
  - database
tags:
  - Oracle
last_modified_at: 2023-07-21
---
## Oracle이란?

> 미국의 오라클 사에서 만든 관계형 데이터베이스이며, 우리나라 대기업과 공공기관에서 가장 많이 사용하는 데이터 베이스
> 

### Oracle의 버전

- Enterprise(ORCL), Standard, Express edition(XE)
- 학습 시에는 Express edition이면 충분
- 관리 영역은 Enterprise 버전 사용

### Oracle 설치 방법

1. **운영체제에 직접 설치**
    - MAC은 설치 불가
    - Windows와 Linux 설치 가능하며 보통 Linux에 설치
    - Oracle web site에서 회원가입 후 다운받아 설치
2. **Docker에 설치**
    - Windows는 설치에 제약 없으나 MAC M1, M2의 경우는 별도 프로그램 설치한 후 설치 가능
    - Windows 설치 방법
        - 11g:
            
            ```bash
            # sid는 xe 관리자는 sys, system이고 비번은 oracle
            docker run --name 컨테이너명 -d -p 1521:1521 jaspeen/oracle-xe-11g
            ```
            
        - 21c:
            
            ```bash
            # sid는 xe 관리자는 sys, system이고 비번은 본인 설정
            docker run --name 컨테이너명 -d -p 1521:1521 -e ORACLE_PASSWORD=관리자 비밀번호 gvenzl/oracle-xe
            ```
            
    

### Oracle 내부 접속 방법

- 무선 네트워크 설정
- host: 192.168.0.4
- sid: xe
- 계정: user12
- 비밀번호: user12
- 드라이버는 다운로드
    
    

### Oracle 외부 접속 방법

- host: 1.220.201.108
- port: 1521
- sid: xe
- 계정: user12
- 비밀번호: user12
- 드라이버는 다운로드
    
    

### Oracle과 MySQL의 차이

- 함수명 등의 이름이다름
- Oracle은 FULL OUTER JOIN 가능
- Oracle에서만 계층형 조회 가능
- 페이징 하는 방법이 다름
- Oracle에 분석 통계함수 많음
    
    

### 테이블 확인

- DESC  명령어 불가, database nevigator에서 직접 확인
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1c26642d-133e-4aa3-bc2e-06eebfe1ac5f/Untitled.png)
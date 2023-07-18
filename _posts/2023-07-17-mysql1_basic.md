---
title: "MySQL 접속하기"
excerpt: "Docker 에서 MySQL 실행하기"

categories:
  - database
tags:
  - SQL
last_modified_at: 2023-07-17
---

## MySQL  접속하기

### 1. docker에서 container 생성 후 runnning
    
```bash
docker run --name mysql -dit -e MYSQL_ROOT_PASSWORD=beh1016 -p 3306:3306  mysql
```

### 2. dbeaver 접속 →파일 새로 만들기→dbeaver→ 데이터베이스 연결→ MySQL 선택
    
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4f9ae345-6098-4da1-855b-0047f326b3ff/Untitled.png)
    
### 3. 접속 정보 작성
- serverhost=ip 정보 혹은 localhost
- port 기본 설정은 3306
- username, password
    
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97813d07-c781-4331-9646-4500a0de01bf/Untitled.png)
        

### 4. 처음 연결 시 드라이버 다운 필요
- 드라이버 직접 다운 받아서 설정 가능
### 5. SQL 편집기 새로 생성하여 명령어 작성

---
title:  "서버 초기 설정"
excerpt: "Ubuntu 서버 설치 후 초기 설정 (디스크 마운트, 유저 등록, 포트포워딩) "

categories:
  - Server
tags:
  - Server

last_modified_at: 2020-09-20T
---

### 하드디스크 마운팅

1. Ubuntu 설치 후 부팅
2. 서버에 연결된 디스크 경로 확인 
    
    ```bash
    fdisk -l
    ```
    
3. 디스크 형식 ext4로 포맷하기 (디스크 마운트를 위해서 필요)
    
    ```bash
    sudo mkfs -t ext4 /dev/diskname
    ```
    
4. 디스크 마운트
    
    ```bash
    sudo mount /dev/diskname /media/run
    ```
    
5. 마운트 자동화
    
    ```bash
    # 디스크 uuid 정보 확인
    sudo blkid
    
    # 하드디스크 정보 추가
    sudo nano /etc/fstab
    
    mount -a
    ```


### 유저 등록 및 그룹 생성

1. 사용자 등록 및 확인
    
    ```bash
    sudo adduser username
    sudo nano /etc/passw
    ```
    
2. 그룹 등록 및 확인
    
    ```bash
    sudo groupadd groupname
    sudo nano /etc/group
    ```
    
3. 그룹에 사용자 등록 → /etc/group에서도 등록 가능
    
    ```bash
    sudo gpasswd -a username groupname
    ```

### 포트포워딩 (네트워크)


1. 인터넷 브라우저 통해서 ipTIME 관리자 모드 접속
    
2. 로그인 정보 입력

3. 관리도구 > 고급설정 > NAT/라우터 관리> 포트포워드 설정

4. 포트포워드 규칙 설정하기

    1. 규칙이름→ 구분하기 위한 이름

    2. 내부 IP 주소  → ifconfig 등으로 확인 가능 

    3. 외부포트: 외부 ip 접속 시 포트 설정

    4. 내부포트: 내부 ip  접속 시 포트 설정 (주로 22)



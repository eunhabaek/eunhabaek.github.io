---
title: "Linux 기본 명령어"
excerpt: "Linux를 사용하는데 필요한 기본 명령어를 알아보자."

categories:
  - Shell
tags:
  - Basic
last_modified_at: 2020-08-03
---



리눅스에 필요한 기본 명령어를 알아보자.


**hostname** 
  
 컴퓨터 이름 호출  -i 옵션은 아이피 확인


  
  
**!$**
  
 이전 명령어와 결과 보기
  
  


**ping 8.8.8.8**
  
 인터넷 상태 확인하기
  
  


**ifconfig**
  
 아이피 확인 
  
  


**sudo** 
  
 관리자 권한으로 명령 실행
  
  


**who**
  
 접속 유저확인
  
  


**useradd  [계정이름]**
  
 계정 추가하기
  
  


**userdel  [계정이름]**
  
 계정 삭제하기
  
  


**su [계정이름]**
  
 접속 계정 바꾸기  ex) su root  루트 계정으로 바꾸기
  
  


**passwd** 
  
 계정 암호 재설정
  
  


**man** 
  
 메뉴얼  ex) man hostname
  
  


**history**
  
 사용한 명령어 목록 전부 보기
  
  


**df**
  
 디스크 확인 -h 옵션은 요약해서 보여줌
  
  


**du**
  
 디렉토리의 디스크 사용 확인 -s는 요약해서 보여줌
  
  


**top**
  
 시스템 상황을 실시간으로 모니터링
  
  


**ps** 
  
 현재 세션에서 실행 프로세스 확인 a 옵션은 모든 터미널에서 실행중인 프로세스 -e 옵션은 히스토리
  
  


**ls** 
  
 현재 디렉토리의 목록 리스트 확인 
  
-a : 숨겨진 목록 출력 
  
-l : 자세한 내용 출력
  
-R : 하위 목록 출력
  
-t : 시간순 정렬 (기본은 알파벳순)
  
-S : 크기순 정렬
  
-u : 접근 시간 출력
  
-c : 수정 시간 출력
  
  


**pwd**
  
 현재 디렉토리
  
  


**cd**
  
 디렉토리 변경 ex) ch -  이전 경로, ch~ 홈 경로, cd / 루트 경로
  
  


**shutdown [옵션] [시간] [메시지]**
  
 시스템 종료하기
  
-k : 실제로 시스템 종료하지 않고 사용자들한테 메시지만 전달
  
-r : 종료 후 재시작
  
-h : 종료
  
-c : 이전에 내렸던 shutdown 명령을 취소
  
시간 : 종료할 시간 (hh:mm, +m, now
  
ex)
  
shutdown -h 12:00  12시에 종료
  
shutdown -r +30 30분 후에 재부팅      
  
  


**;** 
  
 명령어 구분 가능 ex ) who;date;ifconfig
  
  


**!**
  
 이전명령어 불러오기 ex) !p 이전명령어의 p로 시작하는것
  
  


<https://goddaehee.tistory.com/83](https://goddaehee.tistory.com/83>

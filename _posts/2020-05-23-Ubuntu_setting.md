---
title: "Ubuntu 서버 세팅"
excerpt: "ubuntu 서버 환경 구축 방법"

categories:
  - Server
tags:
  - Server
last_modified_at: 2020-05-23
---

- 1  Bootable Ubuntu USB 만들기 (Ubuntu 설치 USB)
    a. 빈 USB 준비
        - 모든 데이터가 삭제되므로 백업을 해야 함
        - 최소 2G 이상 용량
    b. Ubuntu OS 다운로드
        - 원하는 버전의 Ubuntu ISO [다운받기](https://releases.ubuntu.com/) ([https://releases.ubuntu.com/](https://releases.ubuntu.com/))
    c. Bootable USB 만드는 프로그램 설치
        - 공식적으로 ubuntu에서 제시하는 프로그램은 rufus이다. ([https://rufus.ie/](https://rufus.ie/)) 아래와 같이 부팅시킬 USB 장치와 다운받은 ISO 파일을 선택한 후 포맷을 하여 Bootable USB를 만든다.
            
            ![figure1](/figures/2020-05-23-1.png)
            
- 2  Ubuntu 설치
    a. 서버에 USB 연결 후 BIOS에 접속한다.
        - F2, F10, F12등 해당하는 키를 연타하여 BIOS에 접속한다.
    b. BIOS에서 부팅 순서를 USB를 최우선으로 바꾼다. 
    c. Ubuntu 프로그램 설치를 진행한다.

- 3  한영키설정
리눅스를 설치 할 때 영어로 까는 것이 좋다. 그래서 처음 리눅스를 영어로 설치 한 후에는 한글을 쓸 수도 없고 키보드에 있는 한영키도 작동하지 않는 것을 볼 수 있다. 따라서 한영 키 설정을 새로 해 주어야 한다. 

 Settings → Region & Language 에 들어가면 아래와 같이 언어가 영어로 설정되어 있음을 확인 할 수 있다.

![figure2](/figures/2020-05-23-2.png)

 한글과 영어를 한영키로 자유롭게 사용하기 위해서 ibus에서 제공하는 korean (Hangul)을 적용해 보도록 하자.

 먼저 터미널 창을 켠다(Ctrl + Alt + T) 그리고 ibus-hangul을 다운받기 위해 명령어를 입력한다. 

```bash
sudo apt-get install ibus ibus-hangul
```

 그리고 input method 라는 기본적으로 깔려있는 프로그램 창을 띄운다

![figure3](/figures/2020-05-23-3.png)

 

아래와 같은 창이 뜨면 OK를 클릭한다. 

![figure4](/figures/2020-05-23-4.png)

 

이어서 yes를 클릭한다.

![figure5](/figures/2020-05-23-5.png)

 

아래와 같은 창이 나타나면 옵션 중 ibus를 선택하고 OK를 눌러 창을 닫는다.

![figure6](/figures/2020-05-23-6.png)
 

이제 다시 Region & Language로 돌아와서 Input source에서 korean (Hangul)을 추가해 보자. 

![figure7](/figures/2020-05-23-7.png)

![figure8](/figures/2020-05-23-8.png)

 이제 자유롭게 한영키로 한영 전환이 가능하다.
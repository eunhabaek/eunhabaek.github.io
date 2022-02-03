---
title:  "CentOS server 이용방법"
excerpt: "CentOS 서버 이용방법을 알아보자"

categories:
  - Server
tags:
  - Server

last_modified_at: 2020-06-02T
---


scRNAseq Data를 다운받기 위해서 산학 서버(centOS server) 에 계정을 만들었고, 사용해 봤다.

명령어로는 ssh를 사용한다.

```bash
shh -p [num] eunha@[ip]
```

 비밀번호를 입력해서 내 계정으로 들어가면 master 서버로 연결된다. 서버는 내 로컬 컴퓨터와는 별개의 공간이기 때문에 프로그램을 새로 깔아야 한다. 여기서 주의할 점은 master 서버는 학교 인터넷을 사용해서 내 로컬과 산학 서버를 연결해주기 때문에 인터넷을 사용할 수 있지만, 지금부터 설명 할 node에서는 인터넷을 쓸 수 없다. 

 노드에는 anode와 inode가 있는데 anode는 속도는 느리지만 용량이 커서 큰 데이터를 간단하게 다룰 때 사용하고 inode는 용량이 작지만 빠르기 때문에 복잡한 명령어를 쓸 때 주로 사용한다. 상황에 맞추어서 노드를 배정하면 되는데, master에서 node로 로그인 하는 명령어는 qlogin이고, node로 잡을 날리는 명령어는 qsub 이다. 로그아웃은 문자 그대로 logout을 사용하면 된다.

```bash
qlogin -q anode64.q@anode64-1 -pe pe_slots 12
qsub fastq-dump.sh -q anode64.q@anode64-1 -pe pe_slots 12
```


 옵션 -q 를 이용하여 원하는 노드를 선택하고 -pe로 슬롯 개수를 정할 수 있다.  .sh로 끝나는 파일은 bash 파일이라고 하는데 리눅스에서 반복적인 코드를 실행할 때 중요하게 쓰이는 쉘 스크립트이다. bash 파일은 vi, vim 또는 nano 등으로 만들면 되는데 파일 이름 형식은 .sh로 끝나게 해야하고 내용의 맨 첫 줄에도 형식이 있다.  

```bash
#! /bin/bash
#$ -S /bin/bash
#$ -N  fastq-dump
#$ -e /storage/home/eunha/log
#$ -o /storage/home/eunha/log 
#$ -pe pe_slots 12

source /storage/home/eunha/.bash_profile

cd /storage/home/eunha
```

 맨 첫 줄의 #! /bin/bash 이 bash 파일을 인식할 수 있게 해 주므로 잊어서는 안되며, 그 아래 줄들은 fastq-dump bash 파일을 돌릴 때 필요한 명령어들이다. bash파일은 bash, sh 또는 ./  명령어로 실행 가능하다.

```bash
bash test.sh
./test.sh
sh test.sh
```

 PATH 설정을 하지 않은 경우에 명령어 실행에 실패할 수 있으므로 bash 파일을 돌리기 전에 경로를 설정해야 한다. 이때 source는 경로가 저장되어있는 .bash_profile을 불러오게 해 주는데 이 .bash_profile은 ls 명령어로는 볼 수가 없다.  보고 싶으면 ls -a 옵션을 달아주면 되고, 편집할 때는 역시 vi, vim, nano등의 명령어를 사용한다. 아래는 .bash_profile 예시이다. 필요에 따라 경로를 추가해주면 된다.

```bash
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:/storage2/Project/source/gcc8/R363/bin
PATH=$PATH:$HOME/bin
PATH=$PATH:/storage/home/eunha/source/sratoolkit/bin
PATH=$PATH:/storage/home/eunha/source/opt/glibc-2.14/lib

export PATH
```

 이렇게 하면 서버에서 bash파일을 돌릴 수 있다. 이렇게 할당시킨 내 잡들이 잘 돌아가고 있나 확인하고 싶다면 qstat을 사용한다. 옵션 -f를 사용하면 모든 노드를 볼 수 있고  -u는 노드를 사용하는 유저들도 같이 보여준다. 잡을 할당하거나 qlogin을 하기 전에 qstat을 사용해서 다른 유저들과 겹치지 않게 노드를 사용하는 것이 좋다. 

```bash
qstat -f -u "*" 
```

 만약 내가 실행시킨 잡에 문제가 있거나 실행을 종료하고 싶다면 qdel 명령어를 사용하면 되는데 잡 넘버를 입력하면 실행이 종료된다. 여러명이 같이 사용하다 보니 문제가 생겼을 경우에는 바로 바로 종료시켜 주는 것이 좋다.

```bash
qdel 27012
```

 마지막으로 master서버나 qlogin 상태에서 나오기 위해서는 logout 명령어를 사용하면 된다.
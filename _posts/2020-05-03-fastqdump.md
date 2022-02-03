---
title: "SRA-Toolkit로 fastq 다운받기"
excerpt: "sra number를 이용하여 fastq data를 다운받을 수 있다."

categories:
  - Tools
tags:
  - datahandling
last_modified_at: 2020-05-03
---
SRA-Toolkit의 fastq-dump는 sra 파일을 fastq로 바꾸어 준다. 

 대부분의 DB에서 data에 대한 SRA 넘버를 제공하고, SRA에 대량의 data가 있기 때문에 이 툴을 이용한다면 SRA/SRR 넘버로 손쉽게 원하는 data의 fastq를 다운받을 수 있다. SRA-Toolkit에는 fastq 다운 외에도 다른 다양한 기능이 있으니 필요한 경우 찾아보자.

 SRA-toolkit은 [NCBI 사이트](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software)에서 다운 가능한데 fastq-dump 실행을 위해서 따로 필요한 프로그램이 있을 수도 있다. 에러 메세지를 보고 추가적으로 다운받자. 필자의 경우 glibc_2.14가 필요해서 따로 다운받았더니 실행이 됐다.

```bash
# centos
wget https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.10.8/sratoolkit.2.10.8-centos_linux64.tar.gz
# ubuntu 
wget https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.10.8/sratoolkit.2.10.8-ubuntu64.tar.gz
# macOS
wget https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.10.8/sratoolkit.2.10.8-mac64.tar.gz
# MS windows
wget https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.10.8/sratoolkit.2.10.8-win64.zip
```

 환경에 맞는 버전을 다운 받고 fastq-dump를 실행해 보자. 간단하게 fastq 파일 다운이 가능하다.

```bash
fastq-dump SRR19749
```

 SRR list를 만들어서 bash file로 fastq-dump를 돌릴수도 있다. SRR number list를 만들때 [SRA Run Selector](https://www.ncbi.nlm.nih.gov/Traces/study/?o=acc_s%3Aa) 를 이용하면 list를 쉽게 만들 수 있다. bash 파일 예시는 이렇다.

```bash
#!/bin/sh
#$ -S /bin/bash
#$ -N  Fastq-dump
#$ -e /storage/home/eunha/Data/log
#$ -o /storage/home/eunha/Data/log
#$ -pe pe_slots 12

source /storage/home/eunha/.bash_profile

cd /storage/home/eunha/Data

Value=$(<srr_list.txt)

for i in $Value; {
        fastq-dump $i
        echo "$i"
}
```

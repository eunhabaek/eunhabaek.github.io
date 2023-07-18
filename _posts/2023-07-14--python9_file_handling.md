---
title: "Python (9)"
excerpt: "파이썬의 파일 핸들링"

categories:
  - basic
tags:
  - python
last_modified_at: 2023-07-14
---
## 파일 핸들링 ##
#### 1. 파일 처리과정
- open이라는 함수 이용해서 운영 체제에게 파일 사용 할 수 있도록 요청
- 파일 존재하면 참조 리턴, 존재하지 않으면 예외 발생
- 파일에 대한 읽고 쓰기 작업을 운영체제가 수행하면 결과 리턴
- close 함수 이용하여 파일 닫기

#### 2. 파일 쓰기작업
- open 함수에 2, 3개 매개변수 대입하여 파일 열기
    - 첫번째 매개변수는 경로, 두번째는 ‘w’, 세번째는 인코딩 방식(encoding=”utf8”)
- 기록할 때 파일 참조 변수 이용하여 write 호출
- \n 포함된 컬렉션의 경우는 weitelines나 join이용해서 줄 단위 기록이 가능

#### 3. 파일 읽기 작업
- open함수에 1개나 2개의 매개변수 대입해서 파일 열기
    - 첫번째 매개변수는 파일 경로, 두번째 매개변수는 ‘r’
- read( ) 입력 시 전체
- readline( ) 줄 단위
- readlines( ) 줄 단위로 읽어서 리스트로 리턴
- read(정수)는 정수 바이트 만큼 읽기

#### 4. 현재 작업 디렉토리 확인 및 변경
- os.getcwd( ) : 현재 경로 확인
- os.chdir(”path”) : 경로 이동
    ```python   
    import os

    print(os.getcwd()) #현재 작업디렉토리 경로 확인

    try:
        #파일 쓰기
        file=open('./data/test.txt','w',encoding="utf8")
        file.write("hello") #file에 내용 쓰기

        lines=["cats\n","dogs\n","monkeys\n","안녕"]
        file.writelines(lines)

        #파일 읽기
        with open('./data/test.txt','r',encoding="utf8") as file:

            #줄단위로 읽기
            for line in file:
                print(line)
                print()

        #content=file.read() #file 내용 전체 읽기
        #print(content)

    except Exception as e: #예외 처리
        print("파일 읽기 중 에러: ",e)
    ```
## with 절 ##
- with open( ) as 파일변수:
    - open의 리턴 값을 파일 변수로 저장, with 절 끝나면 예외 상관 없이 자동 close( ) 호출
- 파일은 외부 자원이라 작업 끝난 후 close 호출하여 연결 해제 필요.
- 보통 close( )는 finally 구문에 작성

## 파일의 포멧 ##
- txt 파일: 문자열 기록한 파일
    - 텍스트 데이터 저장하는 용도로 사용 가능
    - fwt, fwf: 일정한 간격을 갖는 텍스트
    - tsv: 탭으로 구분한 텍스트
    - csv: 쉼표로 구분한 텍스트
    - ⇒ 최근에는 구분자로 구분된 건 다 csv
    - 불변의 데이터를 csv로 제공↔ 계속 변하는 경우 XML, JSON 제공
- csv 읽기:
    1. 텍스트로 읽어서 split 메서드 이용
        - 문자열 내 따옴표 있는 경우 해석 잘 안됨
        - 마지막 데이터에 \n 추가되어서 제거 필요함
    2. 파이썬이 제공하는 csv 모듈 이용
        - 파일 객체를 csv.reader 객체에 대입하면 줄 단위로 읽을 수 있는 reader  객체 리턴, for로 순회하면 각 줄을 list로 변환해 줌
        - 읽거나 기록 할 때 delimiter 옵션 이용하여 , 아닌 다른 구분자 설정 가능
    3. pandas 같은 외부 라이브러리 이용⇒데이터 프레임 만들어 줌, but heavy

        ```python
        #csv 읽기
        #텍스트로 읽어서 split 이용
        data=[]
        with open('./data/경기도_이천시_지역화폐발행 및 이용현황_20221015.csv','r') as csv1:
            for line in csv1:
                ar=line.split(",")
                data.append(ar)
        print(data)
        ```
    
- csv 쓰기:
    1. csv.writer에 파일 객체 대입하여 csv 파일 쓰기 가능

        ```python
        #csv 쓰기
        import csv
        with open('./data/test_0714.csv','w') as wt:
            wr=csv.writer(wt)
            wr.writerow(["cat","dog","monkey"])
            wr.writerow(["mouse","rabbit","lion"])
        ```
    
- binary 파일과 sericalizable
    - 바이너리 파일
        - 문자열 직접 기록 불가, encode( ) 호출하여 바이트 단위로 기록
        - 문자열로 변환 시 decode
        - 모드 설정 시 +‘b’
        - 텍스트 보다는 이미지 등 저장 시 사용
    - serializable
        - 인스턴스를 파일에 기록하고 읽거나 네트워크 통해서 전송하기 위한 작업
        - 입출력은 기본적으로 byte 단위나 문자단위 가능
        - 사용자 정의 클래스의 인스턴스 단위로 가능
        - 응용 프로그램을 만들고 다른 곳에서 데이터에 접근 제한하기 위함 ex.블랙박스
        - 프로그램으로만 확인 가능
        - pickle 모듈 사용
        - 쓰기 방법:
            - pickle.dump(출력할 객체, 파일 객체)
        - 읽기 방법:
            - pickle.load(파일 객체) : 객체 1개 읽기
            - pickle.loads(파일 객체): 객체 한번에 읽기

                ```python
                import pickle
                #인스턴스 파일 생성
                try:
                    #serializing
                    with open("./data/data.date","wb") as file:
                        pickle.dump(dto1,file)

                    # 만들어진 파일 deserializing 하지 않으면 읽지 못함
                    with open("./data/data.date", "rb") as file:
                        result=pickle.load(file)
                        print(result)

                except Exception as e:
                    print(e)
                ```

## 파일 압축 기능 ##
- zip 압축
    - zipfile 모듈의 ZipFile 함수 이용하여 인스턴스 생성
    - 파일 경로를 list로 만든 후 write 함수 대입하면 압축
    - zip 해제는 extractcall 함수 호출
        ```python
        ##zipfile 압축/해제
        import zipfile

        #압축할 데이터 리스트 만들기
        filelst=['./data/test_0714.bin','./data/test_0714.csv']

        #zip 압축하기
        with zipfile.ZipFile('./data/test_0714.zip','w') as myzipf:
            for f in filelst:
                myzipf.write(f)
        #zip 압축 해제
        zipfile.ZipFile('/data/test_0714.zip').extractall()
        ```
- tar 압축 (리눅스 표준)

## 로그파일 읽기 ##
- Apache Tomcat의 로그 파일
    - 180.76.15.152 - - [15/Nov/2015:09:12:04 +0000] "GET / HTTP/1.1" 200 6812
    - 공백 9개로 분리, 실제 데이터는 10개
        ```python
        #ip별 트래픽 수
        logs={}
        with open('./data/log.txt','r') as log:
            for i in log:
                sub=i.strip("\n").split(' ')
                try:
                    logs[sub[0]]=logs.get(sub[0],0)+int(sub[9])
                except Exception as e:
                    print(e)
        print(logs)
        ```
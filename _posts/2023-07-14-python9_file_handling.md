---
title: "Python (8)"
excerpt: "파이썬의 파일 핸들링"

categories:
  - python
tags:
  - python
last_modified_at: 2023-07-14
---


    ```python
    ```

**모듈/패키지**

- 파이썬의 작업 단위
    1. Function
    2. Class와 Instance
    3. Module ⇒ “하나의 파일”
    4. Package : 배포 단위, 모듈의 집합
- 모듈/패키지 구성요소 사용 방법
    - import 모듈/패키지명
        - 모듈이나 패키지를 현재 프로젝트로 가져옴
            - ex) import math
        - “모듈/패키지명.구성요소명” 형식으로 사용
            - ex) math.abs( )
    - from 모듈/패키지명 import 구성요소
        - 구성요소만 현재 모듈에 추가
            - ex) from math import abs
        - 구성요소 이름으로 사용 가능, * 사용 시 모든 구성요소 사용 가능
            - ex) abs( )
    - from 모듈/패키지명 as 다른 이름
        - 모듈/패키지 이름을 바꾸어서 현재 프로젝트로 가져옴
            - ex) import pandas as pd
    - 모듈/패키지명이 모두 문자열로 되어있는 경우 __import__(”모듈/패키지명”)로 사용 가능
- 모듈/패키지의 공유
    - 한번만 import 하면 모든 곳에서 사용 가능
- 현재 사용 가능한 모듈 확인
    - sys.modules  속성 이용
- 모듈을 찾아오는 위치 확인
    - sys.path 속성 이용
    - sys.path.append(”추가할 경로”) 로 환경변수 추가 가능
- 모듈적 프로그래밍
    - 파이썬 파일 만들고 import

**패키지 생성**

- 패키지는 모듈의 집합으로, 디렉토리와 유사
- 파이썬에서는 디렉토리에 __init__.py 있을 경우 패키지로 간주

**패키지 설치**

- 설치방법 (터미널)
    - pip install package명 : 설치
    - pip install package명 -upgrade : 패키지 업그레이드
    - pip install package명 =버전 : 특정 버전 패키지 설치
    - pip install package명 >=버전 : 특정 버전 이상 패키지 설치
- 관리자 계정이 아니어서 설치 안 될 경우 —user 옵션 추가
- anaconda의 경우 pip 대신 conda 사용
- google colab 사용 시 import 구문 위에 !pip install package명
- 패키지 목록 확인
    - pip list —format=columns

**수학 관련 모듈**

1. math
    - 수학 관련 모듈
    - sqrt( ), ceil( ), floor( ), abs( ), … 등
2. decimal.Decimal( “문자열 실수”)
    - float보다 정확한 실수 표현 (머신 엡실론)
    - 실수를 문자열로 만들어서 Decimal 이용하면 정확함
3. random
    - 난수 추출 위한 모듈
    - seed( )로 난수표 생성 후 숫자 생성 함수 호출 → 난수표에서 순서대로 숫자 리턴
    - seed 고정하면 random이어도 동일 숫자 리턴
    - 예시
        - seed(None)은 랜덤(현재 시간)
        - random(): 0부터 1 사이 소수 리턴
        - uniform(min,max): 숫자 사이 실수 리턴
        - randint(min,max): 숫자 사이 정수 리턴
        - randrange(start,end,step):
        - gauss(m,sb): 가우스 분포 난수
        - shuffle(sequence): 시퀀스 섞음
        - choice(sequence): 복원 추출
        - sample(sequence,n): 개수만큼 추출
    - 머신러닝 학습 시에는 seed 고정
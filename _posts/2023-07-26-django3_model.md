---
title: "Django의 Model"
excerpt: "Django에서 models.py 작성하기"

categories:
  - Webserver
tags:
  - Django
last_modified_at: 2023-07-26
---

## Django의 model

- 데이터 서비스를 제공하는 layer
- django에서는 models.py에 작성
- model 클래스는 관계형 데이터베이스 table과 1:1 매핑
- model 클래스 안의 속성은 table의 컬럼과 1:1 매핑
- primary key 설정하지 않으면 자동으로 id 컬럼 생성
- 속성 만들 때 클래스 변수로 만들어야 하고 각 속성에 맞는 field 클래스 객체를 생성해서 할당
- field 클래스 객체를 생성해서 할당할 때 옵션 설정, 필수 옵션도 있음, 문자열은 최대길이 지정

## Django model의 필드타입

1. **CharField**
    - 문자열 저장에 사용
    - max_length 옵션으로 최대길이 설정
    - 파생 클래스(하위클래스)로 EmailField, GenericIPAddressField, CommaSeparatedIntegerField, FilePathField, URLField 등이 있음
2. **TextField**
    - 대용량 문자열
3. **IntegerField**
4. **BooleanField**
5. **DataTimeField**
6. **DecimalField**
    - 소수
7. **BinaryField**
    - 바이너리 파일 저장
8. **FileField**
    - 파일 저장
9. **ImageField**
    - 이미지 파일 저장
10. **UUIDField**
    - UUID: 식별 코드로 숫자와 문자가 혼합되어 있음

## Django model의 필드 옵션

- **Null**
    - 널 허용 여부
- **Blank**
    - 필수 여부
    - False 시 필수로 지정
- **Primary**
- **Unique**
- **Default**
- **db_column**
    - 컬럼명을 속성명과 다르게 지정

## 데이터베이스 설정

- settings.py의 DATABASES 항목에서 설정
- https://docs.djangoproject.com/en/4.2/ref/databases/
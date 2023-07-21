---
title: "MongoDB의 Collection"
excerpt: "MongoDB에서 collection 관련 명령어"

categories:
  - database
tags:
  - MongoDB
last_modified_at: 2023-07-21
---

## Collection 작업

### Collection 생성

```bash
#db.createCollection('이름')
db.createCollection('new_collection')
```

### collection 이름 조회

```bash
db.getCollectionNames()
```

### collection 리스트 확인

```bash
show collections
```

### collection 삭제

```bash
db.이름.drop
```

### collection 이름 변경

```bash
db.이름.renameCollection(새이름)
```

### collection 내용 확인

```bash
#db.이름.find()
db.cappedCollection.find()
```

### Capped collection

- 용량 정해두고 저장
- IoT나 Embedded와 같이 용량 제한되어 있는 경우 많이 사용 됨
- 용량 초과되면 먼저 저장된 데이터 삭제 됨
- 일정 용량 넘으면 삭제시키면서 삽입
    
    ```bash
    db.createCollection(이름,{capped:true,size:크기})
    db.cappedCollection.insertOne({x:1})
    ```
    

### 많은 양의 데이터 삽입

```bash
for(i=0;i<100;i++){db.cappedCollection.insertOne({x:i})}
```
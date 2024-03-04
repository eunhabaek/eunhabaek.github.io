---
title: "CloudFront와 Route53 이용 CDN, DNS 구축"

categories:
  - AWS
tags:
  - Network
last_modified_at: 2023-10-26
---


CDN (Content Delivery Network)
---

*   Origin server가 아닌 가까운 edge location으로부터 contents를 다운 받을 수 있음

*   캐싱 가능
  
*   AWS의 CloudFront 서비스로 구축 가능


CloudFront 주의점
---------------

*   업데이트 시 바로 적용이 안되는 경우가 있어서 cache 무력화를 해주지 않으면 24시간 이후 반영됨

CloudFront 배포
-------------

### 1) cloudfront에서 배포 생성

*   원본 도메인에서 연결 할 원본 도메인 선택

*   웹사이트 엔드포인트 선택
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/97e2f3b4-82f7-4385-a54b-deb51011b944)

    

*   뷰어 프로토콜 정책 설정
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/f8cd0cab-12c5-4b25-8cdf-c2874557c22f)

    

*   WAF
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/cbf8561a-95aa-4ad0-9df1-801cc74ea569)


*   기본 값 루트 객체 지정
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/96ed8eab-3dee-4954-90a4-2a208c6808c5)

    

*   배포 종료되면 cloudfront 도메인 이름으로 접속 가능

*   변경사항 바로 반영 위해서는 git acion에 코드 추가 필요
    
        - name: CloudFront 캐시 무력화 설정
                uses: chetan/invalidate-cloudfront-action@v2
                env:
                  DISTRIBUTION: ${{ secrets.AWS_CLOUDFRONT_ID }}
                  PATHS: "/*"
                  AWS_REGION: ${{ secrets.AWS_REGION }}
                  AWS_ACCESS_KEY_ID: ${{ secrets.ACCESS_KEY }}
                  AWS_SECRET_ACCESS_KEY: ${{ secrets.SECRET_KEY }}
    

Route53
-------

*   호스팅 영역 등록

ACM (AWS certification Manager)
-----------------
: HTTPS를 위해 SSL/TSL 인증서를 발급받기

*   도메인 이름 설정
    
    ![image](https://github.com/eunhabaek/eunhabaek.github.io/assets/67853963/18195eeb-1c0b-497b-b9dd-83d75f2cc802)

    

*   요청하면 검증 대기 중 상태가 됨

*   인증서 클릭 후 Route53 레코드 생성

인증서와 cloudfront 연결
------------------

*   일반 → 설정 → 편집에서 연결

*   항목 추가하여 아까 만든 도메인 이름 작성

*   인증서 선택 - 도메인 이름이 맞으면 인증서가 뜸

Route53
-------

*   호스팅 영역에서 레코드 선택
    *   처음 생성 후에는 두개의 레코드 보이고, 인증서 만들었다면 3개가 보임

*   레코드 생성 누른 후 레코드 이름에 하위 도메인 입력 (front)

*   별칭 클릭해서 cloudfront 선택, 리전 선택

*   배포 선택

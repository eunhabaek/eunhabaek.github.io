git action 이용 S3에 리액트 앱 빌드파일 업로드
================================


YAML 파일
-------

    name: CI/CD_AWS_S3
    
    on:
      push:
        branches:
          - main
    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - name: 코드 체크아웃
            uses: actions/checkout@v3
          - name: AWS IAM 사용자 설정
            uses: aws-actions/configure-aws-credentials@v2
            with:
              aws-access-key-id: ${{secrets.ACCESS_KEY}}
              aws-secret-access-key: ${{secrets.SECRET_KEY}}
              aws-region: ap-northeast-1
    
          - name: yarn 설치
            run: npm install -g yarn
      
          - name: 리액트 패키지 설치
            run: yarn install
          
          - name: 리액트 빌드
            run: yarn build
            env:
              CI: false
          
          - name: S3 업로드
            run: aws s3 sync build/ s3://lv3-s3 --acl public-read
            env:
              AWS_ACCESS_KEY_ID: ${{secrets.ACCESS_KEY}}
              AWS_SECRET_ACCESS_KEY: ${{secrets.SECRET_KEY}}

리액트 빌드 에러
---------

[https://sundries-in-myidea.tistory.com/105](https://sundries-in-myidea.tistory.com/105)

[![](git%20action%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%20S3%E1%84%8B%E1%85%A6%20%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A2%E1%86%A8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%A2%E1%86%B8%20%E1%84%87%E1%85%B5%E1%86%AF%E1%84%83%E1%85%B3%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF%20%E1%84%8B%E1%85%A5%E1%86%B8%E1%84%85%E1%85%A9%206593860155124aef9c5bc80a371845fe/Untitled.png)](git%20action%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%20S3%E1%84%8B%E1%85%A6%20%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A2%E1%86%A8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%A2%E1%86%B8%20%E1%84%87%E1%85%B5%E1%86%AF%E1%84%83%E1%85%B3%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF%20%E1%84%8B%E1%85%A5%E1%86%B8%E1%84%85%E1%85%A9%206593860155124aef9c5bc80a371845fe/Untitled.png)

*   local에서는 빌드 되는데 git action에서는 빌드 오류 발생

*   Process.env.CI가 true로 되어있어서 warning 로그를 모두 error로 취급해서 build가 안되는 것으로 예상

*   warning을 없애거나 env.CI를 false로 변경해서 해결 가능할 것으로 보임
    
    *   warning 해결법
        
        [https://velog.io/@rgfdds98/debuging-React-Hook-useEffect-has-a-missing-dependency-fetchMovieData.-Either-include-it-or-remove-the-dependency-array](https://velog.io/@rgfdds98/debuging-React-Hook-useEffect-has-a-missing-dependency-fetchMovieData.-Either-include-it-or-remove-the-dependency-array)
        
        ⇒ 소스 코드 수정이 필요하므로 우선 아래의 방법을 선택
        
    
    *   다음과 같이 빌드 단계의 env.CI를 false로 설정
        
                 - name: 리액트 패키지 설치
                    run: yarn install
                  
                  - name: 리액트 빌드
                    run: yarn build
                    env:
                      CI: false
        

*   성공적으로 빌드 되었음을 확인
    
    [![](git%20action%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%20S3%E1%84%8B%E1%85%A6%20%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A2%E1%86%A8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%A2%E1%86%B8%20%E1%84%87%E1%85%B5%E1%86%AF%E1%84%83%E1%85%B3%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF%20%E1%84%8B%E1%85%A5%E1%86%B8%E1%84%85%E1%85%A9%206593860155124aef9c5bc80a371845fe/Untitled%201.png)](git%20action%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%20S3%E1%84%8B%E1%85%A6%20%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A2%E1%86%A8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%A2%E1%86%B8%20%E1%84%87%E1%85%B5%E1%86%AF%E1%84%83%E1%85%B3%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF%20%E1%84%8B%E1%85%A5%E1%86%B8%E1%84%85%E1%85%A9%206593860155124aef9c5bc80a371845fe/Untitled%201.png)
    
    *   하지만 3분정도 걸리는 문제로 캐싱하는 코드 추가

### 캐싱 추가 후 YAML 파일

    name: REACT_S3_action
    
    on:
      push:
        branches:
          - main
    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - name: 코드 체크아웃
            uses: actions/checkout@v3
          - name: AWS IAM 사용자 설정
            uses: aws-actions/configure-aws-credentials@v2
            with:
              aws-access-key-id: ${{secrets.ACCESS_KEY}}
              aws-secret-access-key: ${{secrets.SECRET_KEY}}
              aws-region: ap-northeast-2
    
          - name: node 버전 설정
            uses: actions/setup-node@v4
            with:
              node-version: 18
    
          - name: 캐싱 작업
            uses: actions/cache@v2
            with:
              path: cache
              key: ${{ runner.OS }}-yarn-${{ hashFiles('**/yarn-lock') }}
              restore-keys: |
                ${{ runner.OS }}-yarn-
    
          - name: yarn 설치
            run: npm install -g yarn
      
          - name: 리액트 패키지 설치
            run: yarn install
        
          - name: 리액트 빌드
            run: yarn build
            env:
              CI: false
          
          - name: S3 업로드
            run: aws s3 sync build/ s3://lv3-s3 --acl public-read
            env:
              AWS_ACCESS_KEY_ID: ${{secrets.ACCESS_KEY}}
              AWS_SECRET_ACCESS_KEY: ${{secrets.SECRET_KEY}}

### 캐싱 후 시간 단축 확인

[![](git%20action%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%20S3%E1%84%8B%E1%85%A6%20%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A2%E1%86%A8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%A2%E1%86%B8%20%E1%84%87%E1%85%B5%E1%86%AF%E1%84%83%E1%85%B3%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF%20%E1%84%8B%E1%85%A5%E1%86%B8%E1%84%85%E1%85%A9%206593860155124aef9c5bc80a371845fe/Untitled%202.png)](git%20action%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%20S3%E1%84%8B%E1%85%A6%20%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A2%E1%86%A8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%A2%E1%86%B8%20%E1%84%87%E1%85%B5%E1%86%AF%E1%84%83%E1%85%B3%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF%20%E1%84%8B%E1%85%A5%E1%86%B8%E1%84%85%E1%85%A9%206593860155124aef9c5bc80a371845fe/Untitled%202.png)

### CloudFront 캐싱 무력화 설정

    - name: CloudFront 캐시 무력화 설정
            uses: chetan/invalidate-cloudfront-action@v2
            env:
              DISTRIBUTION: ${{ secrets.AWS_CLOUDFRONT_ID }}
              PATHS: "/*"
              AWS_REGION: ${{ secrets.AWS_REGION }}
              AWS_ACCESS_KEY_ID: ${{ secrets.ACCESS_KEY }}
              AWS_SECRET_ACCESS_KEY: ${{ secrets.SECRET_KEY }}
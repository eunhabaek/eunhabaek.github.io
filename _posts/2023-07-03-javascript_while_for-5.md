---
title: "Javascript (5)"
excerpt: "while 반복문과 for 반복문"

categories:
  - frontend
tags:
  - javascript
last_modified_at: 2023-07-03
--- 
## 1. while ##
- 형식: while(boolean 표현식){  
    표현식이 falsy가 아니면 반복할 내용  
    }  

    ```html
    //메뉴 선택하기
    <script>
        //10 이하 홀수 순서대로 출력
        odd=0
        while(odd<5){
            console.log(odd*2+1)
            odd++
        }
    </script>
    ```

    
- 표현식을 확인하면서 반복
## 2. do ~ while ##
- 형식: do{  
    표현식이 falsy가 아니면 반복할 내용  
    }while(boolean 표현식)  
    
- 표현식을 확인해서 반복
- while과의 차이는 처음 수행 유무

## 3. for ##
- while과 유사한 용도, 객체나 배열 순회
- 형식: for (처음만 수행되는 식;판별식을 위한 표현식;두번째부터 수행할 식){  
    표현식이 falsy가 아닌 경우 수행할 내용  
    }  
    
- 세가지 식은 생략 가능, 셋 다 없으(;;)면 무한반복
- for에서 첫번째, 세번째 식에 여러개 수행문 작성 가능 “,”로 구분

    ```html
    <script>
        for(let idx=0,i=0;idx<5&&i<6;idx++,i+=2){
            console.log(idx+"+"+i)
        }
    </script>
    ```
## 4. for ~ in ##
- for(임시변수 in 배열이나 객체){  
    수행할 문장  
    }  
    
- 배열에서는 인덱스가 순서대로 임시변수에 대입, 개체의 경우는 속성이 임시변수에 대입
- 순회 (iteration): 0개 이상의 데이터를 가진 collection을 순차적으로 접근, index 불필요
 ```html
    <script>
        animals=["cat","dog","rabbit"]
        for (ams in animals){
            console.log(ams)//인덱스 출력
            console.log(animals[ams]) // 배열 요소 출력
        }
    </script>
 ```

## 5. 기타 ##
- break: 반복문 종료
- continue: 다음 반복으로 넘어가기
- return
    - 현재 모듈 수행 종료
    - 호출 지점으로 돌아가기
    - 진행 지점 저장가능
    - return 없으면(void) 못돌아감
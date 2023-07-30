---
title: "React에서 컴포넌트 출력하기"
excerpt: "React에서 props와 state 활용"

categories:
  - database
tags:
  - React
last_modified_at: 2023-07-28
---

## Props

> **상위 컴포넌트가 하위 컴포넌트로 전달하는 데이터**
> 
- 상위 컴포넌트에서 하위 컴포넌트를 출력할 때 이름=값을 같이 기재하면 하위 컴포넌트에서는 this.props.이름으로 props 사용 가능함

### 1. 클래스형 컴포넌트에서 Props 사용

- App.js에서 MyCompo에게 데이터 전달 및 출력 예제
    - App.js 에서 하위 컴포넌트 속성 삽입
        
        ```jsx
        <h1><MyCompo name="eunha"/></h1>
        ```
        
    - MyComponent.jsx 수정
        
        ```jsx
        import React, {Component} from "react";
        
        class MyCompo extends Component{
            render(){
                return(
                    <div>내 이름은 {this.props.name}</div>
                )
            }
        }
        export default MyCompo;
        ```
        
    - 화면 출력 확인
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1c2c0a0d-fad9-409b-80a3-9639e62521cf/Untitled.png)
        

### 2. 함수형 컴포넌트에서 Props 이용

- 함수형에서 props 사용 할 때는 함수 만들 시 매개변수에 이름 기재해서 props 전체를 매개변수로 받아오도록 함
    - App.js에서 컴포넌트 속성 삽입
        
        ```jsx
        <h1><Sample name="은하"/></h1>
        ```
        
    - Sample.jsx 수정⇒ 함수는 class의 메서드가 아니므로 this 쓸 수 없음
        
        ```jsx
        function Sample(props){
            return(<div>Functional Component={props.name}</div>)
        }
        export default Sample;
        ```
        
    - 화면 확인
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bd76012a-5cf2-4fa3-ac80-0d64255d0f6f/Untitled.png)
        

### 3. 하위 컴포넌트 태그 사이의 데이터 문자열

- 하위 컴초넌트에서 자신의 태그 사이 입려된 문자열을 읽고자 할 때는 props.children
    - App.js에서 태그 사이에 문자열 삽입
        
        ```jsx
        <h1><Sample name="은하"/>문자열 출력</h1>
        ```
        
    - Sample.jsx 파일 수정⇒ 태그 사이 삽입된 문자열은 children 속성으로 사용 가능함
        
        ```jsx
        function Sample(props){
            //자바스크립트의 구조 분해 => 미리 할당해놓으면 빠름
            //실제 속성 명과 동일해야 적용 가능함
        		const {name, children}=props;
            
            return(<div>Functional Component={name}
            <br/>태그 안의 내용은 {children}</div>)  
        }
        export default Sample;
        ```
        
    - 화면 출력
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b90e3b8b-ba1d-497b-8558-615655e5a1de/Untitled.png)
        

## State

> **component 내에서 읽고 쓰는게 가능한 데이터**
> 
- 하위 컴포넌트에서는 props 값을 변경 할 수 없어서 생겨난 데이터
- 기본 값을 설정해야만 사용할 수 있고 메서드 통해서만 수정 가능

### State 생성 및 초기 값 설정

- 클래스형 컴포넌트에서는 constructor이용
    - 초기화
        - this.state={state명1:초기값1, state명2: 초기값2,…}
    - 수정
        - this.setState({state명1:,수정값1, state명2: 수정값2,…})
- 함수형 컴포넌트에서는 useState에서 생성과 초기화

### 객체나 배열로 된 State 수정

- 반드시 복사본 만들어 수정한 후 원본에 대입해야 함
- React는 원본 보관 후 원본에 변화가 생기면 변화된 부분을 재출력 하여 동작, 원본에 직접 수정 가할 시 에러 발생
- 원본과 변경 사항 비교 위해서 복사작업 필요함

## Event Handling

- 이벤트 발생 시 수행하는 동작
- 이벤트 명은 on으로 시작, 함수를 대입하여 생성

### Input  내용 변경 시 변경내용 콘솔출력 및 state에 저장

- src/EventCompo.jsx 파일 생성
    
    ```jsx
    import React, {Component} from "react";
    
    class EventCompo extends Component{
        //content라는 state 생성
        state={
            content:''
        }
        render(){
            return(
            <div>
            <h1>Event!</h1>
            <input type="text"
            onChange={
                (e)=>{
                console.log(e.target.value)
                this.setState({content:e.target.value})
                }
            }/>
            </div>
            )
        }
    }
    export default EventCompo;
    ```
    
- App.js에서 import 하여 출력
    
    ```jsx
    import EventCompo from './EventCompo';
    
    function App() {
    return (
        <>
        <EventCompo/>
    		</>);
    }
    export default App;
    ```
    
- 출력 화면에서 이벤트 작성하여 콘솔 확인
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a7d879dd-f6c8-4b14-98c6-a37a6c07413c/Untitled.png)
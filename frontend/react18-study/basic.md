# Basic

### JSX

* JSX(JavaScript XML)는 Javascript에 XML을 추가한 확장한 문법
* Babel을 사용하여 일반 자바스크립트 형태 코드로 변환
* Babel: 크로스 브라우징 (브라우저 호환성) 이슈를 해결하기 위해 생겨난 도구
  * ES6+ 버전의 자바스크립트나 타입스크립트, JSX 등 다른 언어로 분류되는 언어들에 대해서도 모든 브라우저에서 동작할 수 있도록 ES5로 트랜스파일
* 기본 규칙

```jsx
import React from "react";
import Hello from "./hello";
import './App.css';
  
function App() {
 const name = 'react'
 const style = {
    backgroundColor: 'black', // background-color 처럼 - 로 구분되어 있는 이름은 camelCase 형태로 네이밍 해줘야 한다.
    color: 'aqua',
    fontSize: 24, // 기본 단위 px
    paddubg: '1rem', // 다른 단위 사용 시 문자열로 설정
 }
  
 return (
    // JSX 문법상 두 개 이상의 태그는 무조건 하나의 태그로 감싸있어져야 한다.
    <>
        <Hello
        // 열리는 태그 내부에서는 이렇게 주석을 작성 할 수 있습니다.
        />
        {/* 모든 태그는 꼭 닫혀 있어야 한다. */}
        <input />
        {/* JSX 안에 자바스크립트 값을 사용 할 경우 {}로 감싸야 한다. */}
        <div style={style}>{name}</div>
        {/* CSS class 를 설정 할 때에는 class= 가 아닌 className= 으로 설정해줘야 한다. */}
        <div className="gray-box"></div>
    </>
 );
}
  
export default App;
```

React Element 렌더링

* React Element는 component의 구성 요소로, React 앱의 가장 작은 단위
* React Element는 불변의 객체 이므로 생성 이후에는 자식이나 속성을 변경할 수 없음. (ui를 업데이트하려면 새로운 엘리먼트를 생성하고 root.render()로 전달해야 한다.)

### Component

* UI 재사용을 위한 조각 (최소한의 단위)
* 컴퍼넌트 이름은 항상 대문자로 시작 (리엑트는 소문자 컴퍼넌트를 DOM 태그로 인식)

#### 컴포넌트 종류 <a href="#component-and-props" id="component-and-props"></a>

* 2019년 v16.8 부터 함수형 component에 리액트 훅(hook)을 지원해 주어서 현재는 공식 문서에서 함수형 component 와 hook을 함께 사용할 것을 권장.
* 함수형 component 를  사용하는 추세지만 함수형 컴퍼넌트로 처리되지 않는 케이스가 존재하여 클래스형 컴퍼넌트와 함께 사용되기도 함.

### Props

* Props는 부모 component -> 자식 component의 데이터를 전달할 수 있게 해줌.
* props는 단방향으로 읽기 전용
  * props를 받는 자식 component에서 변경하고자 하면, 자식 component의 state로 사용해야 함. useState(prop) 인자로 넣어준 후 setState로 업데이트

#### defaultProps 설정 <a href="#component-and-props-defaultprops" id="component-and-props-defaultprops"></a>

```jsx
// 클래스형 컴포넌트에서 설정 방
import React, { Component } from 'react';
 
class App extends Component {
  //설정 방법 1
  static defaultProps = {
    name: "Chanmin",
    age: 25,
  };
 
 
  render() {
   return();
  }
}
 
//설정 방법 2
App.defaultProps = {
  name: "Chanmin",
  age: 25,
}
```

```jsx
// 함수형 component에서는 구조 분해 할당 문법으로 설
const App = ({ name = "Chanmin", age = 25 }) => {
  return (
    <>
      <h1>이름 : {name}</h1>
      <h3>나이 : {age}</h3>
    </>
  )
}
```

#### propTypes 설정

* 런타임에서 변수에 정해진 type이 아닌 type으로 값이 흘러들어온 경우 에러 발생
* prop-types 라이브러리 사용
* 참고:  [https://ko.reactjs.org/docs/typechecking-with-proptypes.html](https://ko.reactjs.org/docs/typechecking-with-proptypes.html)

#### Props.children

* 어떤 컴포넌트들은 어떤 자식 엘리먼트가 들어올지 미리 예상할 수 없는 경우가 있음\
  (예로 ‘박스’ 역할을 하는 LNB Sidebar 혹은 Modal와 같은 컴포넌트에서 사용)

```jsx
// { children } 인자로 받
export default function Alert ({children}) {
  return (
    <AlertRoot>
      ...
        <AlertMessage>
        /* 컴포넌트 태그 사이에 정의 */
            {children}
        </AlertMessage>
    </AlertRoot>
  ):
}
```

* React에서 제공하는 React.Chlidren 으로 children의 sort, filter, slice 같은 조작이 가능.



### Ref

* Ref는 render 메서드로 생성된 dom 노드나 react element 에 접근하도록 제공
* 무분별한 ref 사용 자제
* Ref의 바람직한 사용 사례
  * 포커스, 텍스트 선택영역, 혹은 미디어의 재생을 관리할 때.
  * 애니메이션을 직접적으로 실행시킬 때.
  * 서드 파티 DOM 라이브러리를 React와 같이 사용할 때.

\



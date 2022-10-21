---
description: React 16.8버전에 새로 추가된 기능
---

# Hooks

### React Hook

지난 React 버전에서는 <mark style="color:red;"></mark> <mark style="color:red;"></mark><mark style="color:red;">**상태 관리 어려움**</mark>과 ** **<mark style="color:blue;">**lifeCycle method**</mark>**로 인해 관련있는 코드들의 분리 문제**가 발생하였고, 이를 해결하기 위해서 React 16.8버전부터 Hook 추가됨.

함수형 컴포넌트에서도 사용할 수 있게 상태 관리와 lifeCycle method(props, state, context, refs, lifecycle)에 대해 직관적인 API 제공



### [등장배경](https://ko.reactjs.org/docs/hooks-intro.html)



* 기존 클래스형 컴포넌트에서 공통 로직을 적용해야 한다면 render props 또는 HOC를 사용해야 하는데, 여러 컴포넌트가 겹치는 경우 wraper hell (component tree depth가 길어짐) 초래하고 코드 추적이 어려워진다.
  * [HOC(High Order Component)](https://ko.reactjs.org/docs/higher-order-components.html): 재사용 가능한 로직을 분리하여 컴포넌트(wrapper)로 만들고, 재사용 불가능한 부분은 wrapped component를 파라미터로 받아서 처리
  * 문제의 요점은 상태 관련 로직을 공유하기 위해 더 좋은 방법이 필요
  * Hook을 사용하면 상태 관련 로직을 추상화할 수 있다.&#x20;
    * Wrapper 컴포넌트 감소하여 코드가 간결해져서 가독성이 좋아진다.
    * 또한 독립적인 테스트와 재사용이 가능하다.
* Lifecycle method에 서로 연관 없는 로직이 작성되기도 하고, 연관 있는(비슷한 관심사를 가진) 로직은 다른 method에 분리돼서 작성되는 경우 버그가 쉽게 발생하고 무결성을 해친다.
  * 상태 관리 라이브러리를 결합하기도 했지만, 이는 너무 많은 추상화를 하고 서로 다른 파일들 사이를 건너뛰기를 요구하여 컴포넌트 재 사용을 더욱 어렵게 한다.
  * 위 문제를 해결하기 위해서, method를 기반으로 쪼개는 것 보다는, Hook을 통해 서로 연관있는 작은 함수의 묶음으로 컴포넌트를 나누는 방법을 사용한다.  로직의 추적을 쉽게 할 수 있도록 reducer를 활용해 컴포넌트의 지역 상태 값을 관리할 수 있다.
* [규칙](https://ko.reactjs.org/docs/hooks-rules.html) (두 규칙을 강제하는 eslint-plugin-react-hooks 플러그인)
  1. 최상위에서만 Hook을 호출
     * 반복문, 조건문 혹은 중첩된 함수 내에서 Hook 호출 금지
     * 작성 순서대로 위에서 아래로 호출됨
  2. React 함수 내에서 hook 호출





### 기본 Hooks 종류

| 이름         | 한 줄 설명 (역할 / 인자 / 반환값)                                         |
| ---------- | -------------------------------------------------------------- |
| useState   | state 관리 hook / 초기 값 / 현재 state와 state 업데이트 함수                 |
| useEffect  | rending 이후 실행해야할 작업 처리 / side effect 수행 함수, dependency list /  |
| useContext | 전역 state 관리 hook / React context 객체 / 그 context의 현재 값          |

1. <mark style="color:red;">**useState: state 관리 hook**</mark>, 초기 값 인자로 받아, 현재 state와 state 업데이트 함수 반환
2. <mark style="color:green;">**useEffect: rending 이후 실행해야할 side effect 작업 처리**</mark>, componentDidMount, componentDidUpdate, componentWillUnmount가 실행되는 시점을 합한 개념
3. <mark style="color:red;">**useContext: 전역 state 관리 hook**</mark>**,** React context 객체를 인자로 받아, 그 context의 현재 값 반환, context는 전역 상태를 저장하고 있고, Context.Provider와 같이 사용
   * React Context
     1. &#x20;React 컴포넌트 트리 안에서 전역적(global)이라고 볼 수 있는 데이터를 공유
     2. 현재 로그인한 유저, 테마, 선호하는 언어 등
     3. context를 이용하면 단계마다 일일이 props를 넘겨주지 않고도 컴포넌트 트리 전체에 데이터를 제공
   * example ([https://www.w3schools.com/react/react\_usecontext.asp](https://www.w3schools.com/react/react\_usecontext.asp))

```jsx
import { useState, createContext, useContext } from "react";
import ReactDOM from "react-dom/client";
 
 
// createContext 함수의 인자에는 상태의 초기값이 들어간다.
const UserContext = createContext();
 
function Component1() {
  const [user, setUser] = useState("Jesse Hall");
 
  return (
    <UserContext.Provider value={user}>
      <h1>{`Hello ${user}!`}</h1>
      <Component2 />
    </UserContext.Provider>
  );
}
 
function Component2() {
  return (
    <>
      <h1>Component 2</h1>
      <Component3 />
    </>
  );
}
 
function Component3() {
  return (
    <>
      <h1>Component 3</h1>
      <Component4 />
    </>
  );
}
 
function Component4() {
  return (
    <>
      <h1>Component 4</h1>
      <Component5 />
    </>
  );
}
 
function Component5() {
  // context의 현재 값은 트리 안에서 이 Hook을 호출하는 컴포넌트에 가장 가까이에 있는 <UserContext.Provider>의 value prop에 의해 결정된다.
  const user = useContext(UserContext);
 
  return (
    <>
      <h1>Component 5</h1>
      <h2>{`Hello ${user} again!`}</h2>
    </>
  );
}
 
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Component1 />);
```

* useContext를 호출한 컴포넌트는 \<MyContext.Provider>의 인자로 전달된 업데이트된 context 값이 변경되면 항상 리렌더링 된다.
* 컴포넌트를 리렌더링 하는 것에 비용이 많이 든다면, 메모이제이션을 사용하여 최적화할 수 있다.
* [https://ko.reactjs.org/docs/context.html](https://ko.reactjs.org/docs/context.html)

### 추가 Hooks

| 이름                                                                                  | 한 줄 설명                                                                                                                                        |
| ----------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| useReducer                                                                          | useState 보다 다양한 값으로 업데이트용 hook / reducer,  초기 값 / 현재 state와 dispatch 함수(action을 발생시키는 함수) 반환                                                  |
| useMemo                                                                             | <p><strong>렌더링 중에 성능 최적화를 위해 특정 값을 재사용 /</strong><br><strong></strong>어떤 연산을 수행하는 함수, dependency list / memoized value 반환</p>                 |
| [useCallback](https://ko.reactjs.org/docs/hooks-reference.html#usecallback)         | <p><strong>처음 렌더링 될때만 callback 함수 생성, 렌더링 중에 특정 함수를 재사용 /</strong> <br><strong>inline callback과</strong> dependency list / memoized 함수 반환</p> |
| useRef                                                                              | ref(특정 DOM element을 참조)를 설정하여, 반환된 객체의.current 값이 실제 element를 가리킨다. 렌더링과 과련없는 값을 관리할때 사용하기도 함                                                 |
| [useLayoutEffect](https://ko.reactjs.org/docs/hooks-reference.html#uselayouteffect) | useEffect와 동일하나, 모든 DOM 변경 후에 동기적으로 rerendering.                                                                                              |



1. <mark style="color:red;">**useReducer: useState 보다 다양한 값으로 업데이트용 hook**</mark>, reducer(현재 state, 업데이트를 위한 action 값 전달 받아 새로운 상태를 반환하는 함수)와 초기 값을 인자로 받음, 현재 state와 dispatch 함수(action을 발생시키는 함수) 반환
   1. Counter를 예로 들어보자\

2. <mark style="color:green;">**useMemo: 렌더링 중에 성능 최적화를 위해 특정 값을 재사용!**</mark> dependency list의 특정 값이 변경되었을 때에만 메모이제이션된 값만 다시 연산을 실행하여 최적화, memoized value 반환
   1. 첫번째 파라미터에는 어떻게 연산할지 정의하는 함수
   2. 두번째 파라미터에는 deps 배열을 넣어주면 되는데, 이 배열 안에 넣은 내용이 바뀌면, 우리가 등록한 함수를 호출해서 값을 연산해주고, 만약에 내용이 바뀌지 않았다면 이전에 연산한 값을 재사용\

3. <mark style="color:green;">**useCallback: 처음 렌더링 될때만 callback 함수 생성, 렌더링 중에 특정 함수를 재사용!**</mark>** ** dependency list의 특정 값이 변경되었을 때에만 callback 함수 생성하여 함수 instnace 생성을 줄여서 최적화, memoized 함수 반환
   * useMemo Hook을 함수에 맞춰서 변형한 hook
4. useRef: ref(특정 DOM element을 참조)를 설정하여, 반환된 객체의.current 값이 실제 element를 가리킨다. 렌더링과 관련없는 값을 관리할때 사용하기도 함
5. <mark style="color:green;">**useLayoutEffect:**</mark> useEffect와 동일, 먼저 useEffect 써보고 안 되면, useLayoutEffect 사용. 모든 DOM 변경 후에 layout을 읽고 painting 전에 수행.



#### UseMemo example Code

```jsx
// 성능 최적화를 위하여 연산된 값을 useMemo라는 Hook 을 사용하여 재사용하는 방법을 알아보도록 하겠습니다.
// App 컴포넌트에서 다음과 같이 countActiveUsers 라는 함수를 만들어서,
// active 값이 true 인 사용자의 수를 세어서 화면에 렌더링
function countActiveUsers(users) {
    // 콘솔에 메시지를 출력하도록 한 이유는, 이 함수가 호출될때마다 우리가 알 수 있게 하기 위해서 작성.
    console.log('활성 사용자 수를 세는중...');
    return users.filter(user => user.active).length;
}
 
 
export default function Hooks() {
  const [users, setUsers] = useState(mockUsers);
  
// const count = countActiveUsers(users);
// 활성 사용자 수를 세는건, users 에 변화가 있을때만 세야되는건데, input 값이 바뀔 때에도 컴포넌트가 리렌더링 되므로 이렇게 불필요할 때에도 호출하여서 자원이 낭비되고 있습니다.
// 이러한 상황에는 useMemo 라는 Hook 함수를 사용하면 성능을 최적화 할 수 있습니다.
// Memo 는 "memoized" 를 의미하는데, 이는, 이전에 계산 한 값을 재사용한다는 의미를 가지고 있습니다.
  const count = useMemo(() => countActiveUsers(users), [users]);
```

#### useRef example code

```jsx
import React, { useRef, useEffect } from "react";
 
function CreateUser({ username, email, onChange, onCreate }) {
  // useRef()를 사용하여 Ref객체를 만들고, 이 객체를 우리가 선택하고 싶은 DOM element에 ref값으로 설정
  // Ref 객체의 .current 값은 우리 원하는 DOM을 가리킨다
  const nameInput = useRef();
  useEffect(() => {
    // input에 focus하는 focus() DOM API 호출
    // nameInput.current.focus();
    nameInput.current.focus();
  }, []);
```

```jsx
export default function Hooks() {
  // Ch1-section12: useRef로 컴포넌트 안의 변수 만들기
  // useRef Hook은 DOM 선택 이외에도, 컴포넌트 안에서 조회, 수정할 수 있는 (렌더링과 관련없는) 변수를 관리하는 것
  // 예를 들어, useRef로 관리하는 nextId의 용도는 배열에 새 항목을 추가할때, 새 항목에서 사용할 고유 id를 관리하는 용도
  // userRef의 파라미터 값은 이 값이 .current 값의 기본 값이며 수정, 조회 가능
  const nextId = useRef(4);
  const [users, setUsers] = useState(mockUsers);
   
  useEffect(() => {
    console.log('컴포넌트가 화면에 나타남');
    return () => {
      console.log('컴포넌트가 화면에서 사라짐');
    };
  }, []);
 
  const onCreate = () => {
    // useState for users
    const newUser = {
        id: nextId.current,
        username,
        email
    };
    //setUsers(users.concat(newUser));
    setUsers([...users, newUser]);
    console.log(users);
    setInputs({
      username: "",
      email: ""
    });
    nextId.current += 1;
  };
```

****

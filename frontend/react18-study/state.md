# State



### **State 란?** <a href="#state-and-lifecycle-state" id="state-and-lifecycle-state"></a>

* props와 유사하지만, 비공개적이고 컴포넌트에 의해서 완전히 제어
* 컴포넌트 안에서 동적인 값을 상태(state)



#### setState() 올바르게 사용하기 3가지 <a href="#state-and-lifecycle-setstate-3" id="state-and-lifecycle-setstate-3"></a>

1. 직접 state를 수정하지 않는다.\
   예를 들어, 이 코드는 컴포넌트를 다시 렌더링하지 않는다.

```jsx
// wrong
this.state.comment = 'Hello';
 
// correct
// 대신에 sestState()를 사용한다. this.state 할당은 constructor() 안에서만 가능하다.
this.setState({comment: 'Hello'});
```

2\. State 업데이트는 비동기적일 수 있다.\
React는 성능을 위해서 여러 `setState()` 호출을 단일 업데이트로 한꺼번에 처리할 수 있다. \
`this.props`와 `this.state`가 비동기적으로 업데이트될 수 있기 때문에, 다음 state를 계산할 때 해당 값을 의존해서는 안 된다.

```jsx
// wrong
// counter update에 실패할 수 있다.
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
 
// correct
// setState()에 객체를 전달하는 것이 아니라 함수를 인자로 전달한다.
// 함수는 이전 state를 첫 번째 인자로 받고,
// 업데이트가 적용된 시점의 props를 두 번째 인자로 받음
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
 
// 화살표 함수, 일반 함수도 가능
this.setState(function(state, props) {
  return {
    counter: state.counter + props.increment
  };
});
```

3\. 클래스형 component에서 state 업데이트를 위해 `setState()`를 호출할 때 React는 제공한 객체를 현재 state로 shallow merge.

```jsx
// 클래스형 component에서 setState()
// state 객체 안에 key로 독립적인 변수 값 포함 가능
constructor(props) {
    super(props);
    this.state = {
      posts: [],    
      comments: []   
    };
}
 
  componentDidMount() {
    fetchPosts().then(response => {
      // 얕은 병합 shallow merge이기 때문에 this.setState({posts})는 this.state.comments에 영향을 주지 않고, this.state.posts 대체한다.
      this.setState({
        posts: response.posts
      });
    });
 
    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```



#### Data는 부모 컴포넌트에서 자식 컴포넌트로 흐른다. <a href="#state-and-lifecycle-data-." id="state-and-lifecycle-data-."></a>

* 하향식(top-down), 단방향식 데이터 흐름: 컴포넌트는 자신의 state를 자식 컴포넌트의 prop로 전달할 수 있다.
* 하지만, 이 local state는 캡슐화되어 있기 때문에 다른 컴포넌트가 접근할 수 없다.

```jsx
<FormattedDate date={this.state.date} />
 
// FormattedDate 컴포넌트는 date를 자신의 props로 받을 것이고
// 이 date는 Clock 컴포넌트의 state로부터 왔는지, Clock의 props에서 왔는지, 수동으로 입력한 것인지 알지 못한다.
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

* data 흐름이 양방향이면 에러 발생시 추적이 어려움
* 모든 state는 항상 특정한 컴포넌트가 소유하고 있고, 그 state로부터 파생된 UI 또는 트리 구조에서 자신의 "아래"에 있는 컴포넌트에만 영향을 미친다.

컴포넌트가 유상태, 무상태에 대한 것은 구현 세부사항

* 유상태 컴포넌트 안에서 무상태 컴포넌트를 사용할 수 있고, 그 반대도 가능
* 요즘 UI component는 props 만 받아서, presentational component 역할을 수행하여 business logic과 분리되는 추

#### **React State (**[**https://ko.reactjs.org/docs/faq-state.html**](https://ko.reactjs.org/docs/faq-state.html)**)ㅊ** <a href="#state-and-lifecycle-reactstate-https-ko.reactjs.org-docs-faq-state.html" id="state-and-lifecycle-reactstate-https-ko.reactjs.org-docs-faq-state.html"></a>

* Props와 State 차이
  * props (“properties”의 줄임말) 와 state 는 일반 JavaScript 객체
  * &#x20;객체 모두 렌더링 결과물에 영향을 주는 정보를 갖고 있는데, props는 (함수 매개변수처럼) 컴포넌트에 전달되는 반면 state는 (함수 내에 선언된 변수처럼) 컴포넌트 안에서 관리
* 주의사항
  * 이전 state 값을 기준으로 변경하고자 할때는 setState() 에 함수를 전달한다.

```jsx
incrementCount() {
  this.setState((state) => {
    // 중요: 값을 업데이트할 때 `this.state` 대신 `state` 값을 읽어옵니다.
    return {count: state.count + 1}
  });
}
 
handleSomething() {
  // `this.state.count`가 0에서 시작한다고 해봅시다.
  this.incrementCount();
  this.incrementCount();
  this.incrementCount();
 
  // 지금 `this.state.count` 값을 읽어 보면 이 값은 여전히 0일 것입니다.
  // 하지만 React가 컴포넌트를 리렌더링하게 되면 이 값은 3이 됩니다.
}
```

Counter.js

```jsx
// https://react.vlpt.us/basic/07-useState.html
 
import React, { useState } from 'react';
 
function Counter() {
  const [number, setNumber] = useState(0);
 
  // onClick event handler 안에서도 number 상태(state) 값을 변경하기 위한 hook인 setNumber의 함수 안에서도 함수로 전달해야 한다.
  // setNumber(number + 1); 이 아니라 setNumber(prevNumber => prevNumber + 1);
  const onIncrease = () => {
    setNumber(prevNumber => prevNumber + 1);
  }
 
  const onDecrease = () => {
    setNumber(prevNumber => prevNumber - 1);
  }
 
  return (
    <div>
      <h1>{number}</h1>
      <button onClick={onIncrease}>+1</button>
      <button onClick={onDecrease}>-1</button>
    </div>
  );
}
 
export default Counter;
```

{% hint style="warning" %}
배열을 업데이트할때는 배열의 내장함수(concat, filter, map..)을 사용, 객체를 업데이트할때는 spread 연산자(...) 사용해서 setState()에 전달해야 한다.

새로운 reference 로 전달해야, React에서 DOM 업데이트 발
{% endhint %}

```jsx
// 객체 업데이트 예시
const onChange = (e) => {
  const { name, value } = e.target;
  setInputs({
    ...inputs, // 기존 input 객체를 복사한 뒤
    [name]: value // name key 를 가진 값을 value로 설정
  });
};
 
// 배열 업데이트 예시
const onCreate = () => {
  const user = {
    id: nextId.current,
    username,
    email
  };
  setUsers(users.concat(user));
};
```

---
description: React 16.8버전에 새로 추가된 기능
---

# Hooks

### React Hook

* state관리, lifecycle 등을 함수형 컴포넌트에서도 사용할 수 있게 한다.
  * props, state, context, refs, lifecycle에 대해 직관적인 API 제공

### [등장배경](https://ko.reactjs.org/docs/hooks-intro.html)

* 기존 클래스형 컴포넌트에서 공통 로직을 적용해야 한다면 render props 또는 HOC 컴포넌트를 사용해야 하는데, 여러 컴포넌트가 겹치는 경우 wraper hell (component tree depth가 길어짐) 초래하고 코드 추적이 어려워진다.
  * [HOC(High Order Component)](https://ko.reactjs.org/docs/higher-order-components.html): 재사용 가능한 로직을 분리하여 컴포넌트(wrapper)로 만들고, 재사용 불가능한 부분은 wrapped component를 파라미터로 받아서 처리
  * 문제의 요점은 상태 관련 로직을 공유하기 위해 더 좋은 방법이 필요
  * Hook을 사용하면 상태 관련 로직을 추상화할 수 있다. Wrapper 컴포넌트 감소하여 **코드가 간결해져서 가독성이 좋아진다. 또한 독립적인 테스트와 재사용이 가능**하다.
* Lifecycle method에 서로 연관 없는 로직이 작성되기도 하고, 연관 있는(비슷한 관심사를 가진) 로직은 다른 method에 분리돼서 작성되는 경우 버그가 쉽게 발생하고 무결성을 해친다.
  * 상태 관리 라이브러리를 결합하기도 했지만, 이는 너무 많은 추상화를 하고 서로 다른 파일들 사이를 건너뛰기를 요구하여 컴포넌트 재 사용을 더욱 어렵게 한다.
  * 위 문제를 해결하기 위해서, method를 기반으로 쪼개는 것 보다는, Hook을 통해 서로 연관있는 작은 함수의 묶음으로 컴포넌트를 나누는 방법을 사용한다. (구독 설정 및 데이터를 불러오는 것과 같은 로직). 로직의 추적을 쉽게 할 수 있도록 reducer를 활용해 컴포넌트의 지역 상태 값을 관리할 수 있다.
* [규칙](https://ko.reactjs.org/docs/hooks-rules.html) (두 규칙을 강제하는 eslint-plugin-react-hooks 플러그인)
  1. 최상위에서만 Hook을 호출
     * 반복문, 조건문 혹은 중첩된 함수 내에서 Hook 호출 금지
     * 작성 순서대로 위에서 아래로 호출됨
  2. React 함수 내에서 hook 호출

### 기본 Hooks 종류

| 이름         | 한 줄 설명                                                                |
| ---------- | --------------------------------------------------------------------- |
| useState   | state 관리 hook, 초기 값 인자로 받아, state와 state 업데이트 함수 반환          |
| useEffect  | rending 이후 실행해야할 작업 처리, componentDidMount, componentDidUpdate, componentWillUnmount가 실행되는 시점을 합한 개념 |
| useContext | React context 객체를 인자로 받아, 그 context의 현재 값 반환, context는 전역 상태를 저장하고 있고, Context.Provider와 같이 사용 |

### 추가 Hooks

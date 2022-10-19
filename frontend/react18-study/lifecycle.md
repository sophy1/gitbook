# LifeCycle

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>React18 에서는 함수형 component를 주로 사용하므로, 위 lifecycle diagram이 일반적이다. <br></p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption><p>위 이미지는 클래스형 component lifecycle diagram. 출처: <a href="https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/">https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/</a></p></figcaption></figure>



* prefix 'WIll': 어떤 작업 작동하기 전에 실행되는 API
* prefix 'Did': 어떤 작업 작동한 후에 실행되는 API
* 크게 3가지로 구분
  * Mount: 페이지에 컴포넌트가 나타남
  * Update(Rerender): 컴포넌트 정보를 업데이트
  * Unmount: 페이지에 컴포넌트가 사라짐
* Mount
  * DOM이 생성되고, 웹 브라우저상에 나타남
  * 호출되는 API
    1. [constructor](https://ko.reactjs.org/docs/react-component.html#constructor): 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자. this.state 초기화
    2. getDerivedStateFromProps: props에 있는 값을 state에 넣을 때 사용
    3. render: UI를 렌더링
       1. this.props, this.state 접근 가능하고, React element를 반환
       2. 아무 것도 보여주고 싶지 않으면, null / false 반환
       3. DOM 정보를 가져오거나 state에 변화를 줄 때는 componentDidMount에서 처리
    4. componentDidMount: 컴포넌트가 웹 브라우저상에 나타난 후 호출
       1. 다른 JS 라이브러리, 또는 프레임워크의 함수를 호출하거나, 이벤트 등록, setTimeout, setInterval, 네트워크 요청같은 비동기 작업 처리
* Update
  * 4가지 요인(props, state, 부모 컴포넌트, this.forceUpdate) 에 의해서 update 발생
  * 호출되는 API
    1. static [getDerivedStateFromProps](https://ko.reactjs.org/docs/react-component.html#static-getderivedstatefromprops): 마운트 과정에서도 호출되며, 업데이트가 시작하기 전에도 호출. props 변화에 따라 state 값에도 변화를 주고 싶을때 사용
    2. shouldComponentUpdate: 성능 최적화를 위해 주로 사용, true이면 다음 lifecycle method 실행하고, false면 작업 중지. 리렌더링을 시키지 않는다.
    3. render: 컴포넌트를 리렌더링
    4. [getSnapshotBeforeUpdate](https://ko.reactjs.org/docs/react-component.html#getsnapshotbeforeupdate): 컴포넌트 변화를 DOM에 반영하기 직전에 호출. 업데이트하기 직전의 값을 참고할 일이 있을 때 활용.(예: 스크롤바 위치 유지)
    5. [componentDidUpdate](https://ko.reactjs.org/docs/react-component.html#componentdidupdate): 컴포넌트의 업데이트 작업이 끝난 후 호출
* Unmount
  * 컴포넌트를 DOM에서 제거
  * 호출되는 API
    * componentWIllUnmount: 컴포넌트가 웹 브라우저 상에서 사라지기 전에 호출
      * 여기서는 주로 DOM에 직접 등록했었던 이벤트를 제거하고, 만약에 setTimeout 을 걸은것이 있다면 clearTimeout 을 통하여 제거
      * 추가적으로, 외부 라이브러리를 사용한게 있고 해당 라이브러리에 dispose 기능이 있다면 여기서 호출
* 에러 처리 \
  [https://ko.reactjs.org/docs/react-component.html#error-boundaries](https://ko.reactjs.org/docs/react-component.html#error-boundaries)



&#x20;

### Ref.

* 자세한 설명은 [https://ko.reactjs.org/docs/react-component.html](https://ko.reactjs.org/docs/react-component.html) 작성됨

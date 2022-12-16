---
description: https://reactrouter.com/en/main/start/overview
---

# Router

## react-router를 이용한 SPA(Single Page Application) 만들기

### 1. SPA란?

* [react-router](https://reactrouter.com/en/main/start/overview)
  * react 내장이 아닌 third-party library로 client side routing을 지원
* SPA 나오게 된 배경
  * MPA로 페이지 마다 서버에서 렌더링하는 건 서버 자원이 사용되는 것이고, 불필요한 트래픽 낭비
  * React, Vue.js를 사용해서 뷰 렌더링을 클라이언트가 담당하고, App 구동에 필요한 자원(css, js, html, image, fonts..)를 브라우저에 로드한 다음에 정말 필요한 데이터만 JSON으로 전달받아 렌더링&#x20;
* SPA에서 Router 필요한 이유
  * SPA는 index.html 파일 하나만 있는데, 앱은 여러 화면이 있어야 한다. 예를 들어 블로그를 만든다면, 홈, 포스트 목록, 포스트 상세 페이지, 포스트 수정 페이지 등의 화면이 필요하다. 또한 이 화면에 접근할 수 있는 주소가 필요하다. 주소가 있어야, 유저들이 북마크도 할 수 있고 서비스에 구글을 통해 유입(SEO)될 수 있다. 다른 path에 따라 다른 화면을 보여주는 것을 라우팅!
  * react 자체에는 이 기능이 내장되어 있지 않아서, 우리가 직접 BOM(Browser Object Model) API 를 사용하고 상태를 설정하여 다른 뷰를 보여주어야 한다. => 편하게 react-router 사용하자
  * 단점
    * public/index.html 보면 id=root 인 element만 존재하고, JS 다운 받은 뒤에 그려주는데, 기능이 많을 수록 앱 규모가 커질 수록 번들링된 JS 파일들 사이즈가 커지게 된다. => Code Spliting을 통해서 트래픽과 로딩 속도 개선, isomorphic JS(SSR + CSR) 혼용해서 사용
    * index.html 초기 렌더링 화면에는 root element만 있기 때문에 SEO에도 좋지 않다.

> 예전에 FE framework 사용 안하고 SPA 구현시에는 location의 hash(anchor) 값에 대한 change handler를 통해서 화면을 그려줬다.&#x20;
>
> [Vue.js routes](https://v3.router.vuejs.org/kr/guide/essentials/history-mode.html)에서는 history mode와 hash mode 가 있는데,  [React Router ](https://reactrouter.com/en/main/routers/create-hash-router)에서는 hash mode를 비추한다. 아무래도 hash mode에는 URL에 #이 붙어서 안 좋게 보이고 SEO 에도 좋지 않기 때문.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### 2. react-router 적용 및 기본 사용법

| <p>TL;DR</p><ol><li>Page Moving(useNavigate)</li><li>Path Variable(useParams)</li><li>Query String(useSearchParams)</li></ol> |
| ----------------------------------------------------------------------------------------------------------------------------- |

#### 2-1. 프로젝트에 라우터 적용

* npm install react-router-dom 라이브러리 설치

{% code title="src/index.js" %}
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
// src/index.js 파일에서 react-router-dom 에 내장되어 있는 BrowserRouter라는 컴포넌트를 사용해서 <App /> 감싸면 된다.
import { BrowserRouter } from 'react-router-dom';


ReactDOM.render(
	<BrowserRouter>
		<App />
	<BrowserRouter>,
	document.getElementById('root')
);
```
{% endcode %}

#### 2-2. 페이지(routes) 컴포넌트 만들기

* src/pages 또는 src/views 또는 src/메뉴 이름.. 에 각 페이지에서 사용할 컴포넌트 생성
  * src/pages/Home.js
  * src/pages/About.js
* 위 컴포넌트의 경로는 프로젝트 개발시 브라우징을 최소화하는 방향으로 구조로 잡고 정해주는 편이다.&#x20;

#### 2-3. Route 컴포넌트로 특정 경로 매핑

* Route 컴포넌트를 통해 특정 경로(path)에 해당하는 컴포넌트를 매핑시켜준다.

{% code title="src/App.js" %}
```jsx
import { Route, Routes } from 'react-router-dom';
import About from './pages/About';
import Home from './pages/Home';


const App = () => {
	return  (
		<Routes>
			<Route path="/" element={<Home />} />
			<Route path="/about" element={<About />} />
		</Routes>
	)
}
```
{% endcode %}

#### 2-4. Link 컴포넌트를 사용하여 다른 페이지로 이동하는 링크 만들기

* (react-router ver.5 이전) 다른 페이지로 이동하는 방법
  * anchor 태그: 브라우저에서 href 페이지를 새로 불러오기 때문에, 다른 사이트로 이동하는 경우
  * \<Link> 컴포넌트: 페이지를 새로 불러오는 것이 아니라, react-router가 내부적으로 History API를 통해 브라우저의 주소 경로만 변경

{% code title="src/pages/Home.js" %}
```jsx
import { Link } from 'react-router-dom';

const Home = () => {
  return (
    <div>
		<h1> 홈이다! </h1>
		<Link to="/about"> 소개로 이동하자! </Link>
	</div>
  );
};

export default Home;
```
{% endcode %}

* (react-router ver.v6 이후)&#x20;
  * **useNavigate() : 특정 조건에 따라 경로를 다르게 줘서 페이지 이동해야 하는 경우.**
    * useNavigate() 훅(Hook)으로 navigate 함수를 가지고 온다.
    * navigate() 함수의 첫번째 인자에 이동할 경로, 두번째 인자의 state 속성에 파라미터를 넣어준다.\
      \=> navigate( '/이동경로', { state: { 키: 값, 키: 값, ... } } )&#x20;

```jsx
import React from "react";
import { useNavigate } from "react-router-dom";

function Login() {
  const navigate = useNavigate();

  // 실제 활용 예시
   const goToMain = () => {
     if(response.message === "valid user"){
       navigate('/main');
     } else {
       alert("가입된 회원이 아닙니다. 회원가입을 먼저 해주세요.")
       navigate('/signup');
     }
  }

  return (
    <>
      <button className="loginBtn" onClick={goToMain}>
        로그인
      </button>
      <!-- navigate() 함수는  useHistory() 포함하고 있다.-->
      <button onClick={() => navigate(-2)}>
        Go 2 pages back
      </button>
      <button onClick={() => navigate(-1)}>
        Go back
      </button>
      <button onClick={() => navigate(1)}>
        Go forward
      </button>
      <button onClick={() => navigate(2)}>
        Go 2 pages forward
      </button>
    </>
  );
}

export default Login;
```

### 3. Path variable와 Query String&#x20;

* 페이지 path를 정의할 때 유동적인 값을 사용하는 경우
  * Path variable: domain 뒤에 **/:resourceId, /:resourceName** 으로 작성&#x20;
    * &#x20;예: /profile**/velopert**
    * 주로 ID 또는 resource 이름을 사용하여 특정 data를 조회할 때 사용
  * Query String: Path 뒤에 ? 뒤로 key=value & 로 연결하여 작성
    * 예: /articles**?page=1\&keyword=react**
    * pagination, search, sort 등 data 조회에 필요한 옵션을 전달할 때 사용

#### 3-1. Path variable(URL parameter)

* **useParams Hook을 사용하여 path variable 객체 타입으로 조회할 수 있다.**
  * react-router에서 제공하는 Hooks 중 하나로 React 16.8 버전 이상에서만 구동이 가능하다.
  * Parameter 값을 URL을 통해서 넘겨서 넘겨받은 페이지(주로 상세 페이지)에서 사용할 수 있도록 도와준다.
  * 예를 들어, 여러 개의 영화 정보가 담겨있는 데이터를 출력해준다고 가정할 때, 영화 제목을 클릭해서 상세 페이지로 이동을 하도록 구현한다면, 영화의 id 값을 URL로 넘겨 상세 페이지에 id 값에 해당하는 영화 정보만 출력하도록 만들 수 있도록 도와준다.
  * path variable은 path 안에 여러 개 작성 가능하다. 예를 들어, /detail/:id/photos/:photoId

1. 먼저 data 객체의 형태를 본다.

{% code title="data.json" %}
```
{
  "data": [
    {
      "id": 0,
      "title": "HTML",
      "description": "HTML (HyperText Markup Language) is the most basic building block of the Web. It defines the meaning and structure of web content. Other technologies besides HTML are generally used to describe a web page's appearance/presentation (CSS) or functionality/behavior (JavaScript)."
    },
    {
      "id": 1,
      "title": "CSS",
      "description": "Cascading Style Sheets (CSS) is a stylesheet language used to describe the presentation of a document written in HTML or XML (including XML dialects such as SVG, MathML or XHTML). CSS describes how elements should be rendered on screen, on paper, in speech, or on other media."
    },
    {
      "id": 2,
      "title": "JavaScript",
      "description": "JavaScript (JS) is a lightweight, interpreted, or just-in-time compiled programming language with first-class functions. While it is most well-known as the scripting language for Web pages, many non-browser environments also use it, such as Node.js, Apache CouchDB and Adobe Acrobat."
    }
  ]
}
```
{% endcode %}

2\. List를 보여줄 컴포넌트(Home)과 Detail 상세 페이지를 보여줄 컴포넌트(Detail)에 대한 Route 정보를 작성한다.

{% code title="src/App.js" %}
```jsx
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import './App.css';
import Home from './routes/Home';
import Detail from './routes/Detail';
import Context from './context/context';

function App() {
  return (
    <Context>
      <Router>
        <Routes>
          <Route path="/" exact element={<Home />} />
          <Route path="/detail/:id" element={<Detail />} />
        </Routes>
      </Router>
    </Context>
  );
}

export default App;
```
{% endcode %}

3\. List 보여줄 컴포넌트

{% code title="src/routes/Home.js" %}
```jsx
import { useEffect, useContext, useState } from 'react';
import { Link } from "react-router-dom";
import { Context } from '../context/context';

export default function Home() {
  const context = useContext(Context);
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch('../data.json')
    .then(res => res.json())
    .then(res => {
      context.data = res.data;
      setData(res.data);
    })
  }, [])

  return (
    <section>
      {data.map(item => (
        <Link to={`/detail/${item.id}`} key={item.id}>
          <h2>{item.title}</h2>
        </Link>
      ))}
    </section>
  )
}
```
{% endcode %}

4\. Detail 컴포넌트

{% code title="src/routes/Detail.js" %}
```jsx
import { useContext } from "react";
import { useParams } from "react-router-dom";
import { Context } from "../context/context";

export default function Detail() {
  const {id} = useParams();
  const context = useContext(Context);
  const langDetail = context.data[id]

  return (
    <section>
      { langDetail ? (
		<div>
     	   <h1>{langDetail.title}</h1>
     	   <p>{langDetail.description}</p>
		</div>
	   ) : ( <p> 존재하지 않음 ! </p> )}
    </section>
  )
}
```
{% endcode %}

#### 3-2. Query String

* Route 컴포넌트에 별도 설정해야 할 게 없음  (\<Route path="/detail/:id" element={\<Detail />} />)
* useLocation  Hook을 사용하여 window.location 객체를 조회할 수 있다.&#x20;
* 하지만, react-router v6 부터는 **useSearchParams Hook을 사용하여 더욱 쉽게 query string을 파싱해서 사용할 수 있다.**
* [https://reactrouter.com/en/main/hooks/use-search-params](https://reactrouter.com/en/main/hooks/use-search-params)

```jsx

import { useSearchParams } from "react-router-dom";

const Edit = () => {
    const [searchParams, setSearchParams] = useSearchParams();

    const id = searchParams.get('id');
    const mode = searchParams.get('mode');
    const onChangeMode = (nextMode) => { setSearchParams( { id, mode: nextMode } ) };
};

```

### 4. Nested route

#### 4-1. 중첩 라우트 사용 예



* 대부분 출시되는 SPA는 라우트 안에 라우트(nested route) 사용한다.&#x20;
* 어떻게?
  * App.js 에서 Route 다 포함시키지 않고 src/routes/  디렉토리 하위로 모듈을 분리한 다음에, App의 index에서는 \<Routes> component 만 포함시킨다.
  * 예) [https://github.com/raminr77/react-sample/tree/main/src/routes](https://github.com/raminr77/react-sample/tree/main/src/routes)
  * [https://github.com/raminr77/react-sample/blob/main/src/routes/index.tsx#L16    \
    https://github.com/raminr77/react-sample/blob/main/src/routes/private-route.tsx#L27](https://github.com/raminr77/react-sample/blob/main/src/routes/index.tsx#L16https://github.com/raminr77/react-sample/blob/main/src/routes/private-route.tsx#L27)
* Path와 React Component 매핑 정보는 routes.js 파일에서 object로 담아서 array에 집어 넣는다.

#### 4-2. \<Outlet> 컴포넌트 ([https://reactrouter.com/en/main/components/outlet](https://reactrouter.com/en/main/components/outlet))

* Nested routes에서 \<Outlet> 페이지는 부모 route elements에서 자식 route elements 를 렌더링할때 보여줄 때 사용한다.
* 예를 들어, GNB, LNB, Footer 는 공통적으로 보여줘야 하는 레이아웃인 경우에 유용하게 사용
* 그래서 뭐가 편한거지?
  * 예를 들어서, Home, About, Profile 페이지에서 상단에 헤더를 보여줘야 하는 상황을 가정해봅시다.
  * 첫 번째로 드는 생각은 아마 Header 컴포넌트를 따로 만들어두고 각 페이지 컴포넌트에서 재사용을 하는 방법일 것입니다. 물론 이 방법이 틀린것은 아니지만, 방금 배운 중첩된 라우트와 Outlet을 활용하여 구현을 할 수도 있습니다.
  * 중첩된 라우트를 사용하는 방식을 사용하면 컴포넌트를 한번만 사용해도 된다는 장점이 있죠.

### 5. react-router 부가 기능

#### 5-1.  NotFound 페이지 만들기

```jsx

import { Route, Routes } from 'react-router-dom';
import Layout from './Layout';
import About from './pages/About';
import Article from './pages/Article';
import Articles from './pages/Articles';
import Home from './pages/Home';
import NotFound from './pages/NotFound';
import Profile from './pages/Profile';

const App = () => {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/profiles/:username" element={<Profile />} />
      </Route>
      <Route path="/articles" element={<Articles />}>
        <Route path=":id" element={<Article />} />
      </Route>
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
};

export default App;
//* 는 wildcard 문자인데, 이는 아무 텍스트나 매칭한다는 뜻
// 이 라우트 엘리먼트의 상단에 위치하는 라우트들의 규칙을 모두 확인하고, 일치하는 라우트가 없다면 이 라우트가 화면에 렌더링
```

#### 5-2. NavLink 컴포넌트

---
description: https://reactrouter.com/en/main/start/overview
---

# Router

## react-router를 이용한 SPA(Single Page Application) 만들기

### 1. SPA란?

* [react-router](https://reactrouter.com/en/main/start/overview)
  * react 내장이 아닌 third-party library로 client side routing을 지원
* SPA
  * 나오게 된 배경: MPA로 페이지 마다 서버에서 렌더링하는 건 서버 자원이 사용되는 것이고, 불필요한 트래픽 낭비
  * React, Vue.js를 사용해서 뷰 렌더링을 클라이언트가 담당하고, App 구동에 필요한 자원(css, js, html, image, fonts..)를 브라우저에 로드한 다음에 정말 필요한 데이터만 JSON으로 전달받아 렌더링
  * Router 필요한 이유:  SPA는 index.html 파일 하나만 있는데, 앱은 여러 화면이 있어야 한다. 예를 들어 블로그를 만든다면, 홈, 포스트 목록, 포스트 상세 페이지, 포스트 수정 페이지 등의 화면이 필요하다. 또한 이 화면에 접근할 수 있는 주소가 필요하다. 주소가 있어야, 유저들이 북마크도 할 수 있고 서비스에 구글을 통해 유입(SEO)될 수 있다. 다른 주소에 따라 다른 화면을 보여주는 것을 라우팅!&#x20;
    * 리액트 자체에는 이 기능이 내장되어 있지 않아서, 우리가 직접 브라우저의 API 를 사용하고 상태를 설정하여 다른 뷰를 보여주어야 한다. => 편하게 react-router 사용하자
  * Cons
    * public/index.html 보면 id=root 인 element만 존재하고, JS 다운 받은 뒤에 그려주는데, 기능이 많을 수록 앱 규모가 커질 수록 번들링된 JS 파일들 사이즈가 커지게 된다. => Code Spliting을 통해서 트래픽과 로딩 속도 개선, isomorphic JS(SSR + CSR) 혼용해서 사용
    * index.html 초기 렌더링 화면에는 root element만  있기 때문에  SEO에도 좋지 않다.

> 예전에 FE framework 사용 안하고 SPA 구현시에는 location의 hash(anchor) 값에 대한 change handler를 통해서 화면을 그려줬다.&#x20;
>
> [Vue.js routes](https://v3.router.vuejs.org/kr/guide/essentials/history-mode.html)에서는 history mode와 hash mode 가 있는데,  [React Router ](https://reactrouter.com/en/main/routers/create-hash-router)에서는 hash mode를 비추한다. 아무래도 hash mode에는 URL에 #이 붙어서 안 좋게 보이고 SEO 에도 좋지 않기 때문.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### 2. react-router 적용 및 기본 사용법

#### 2-1. 프로젝트에 라우터 적용

#### 2-2. 페이지(routes) 컴포넌트 만들기

#### 2-3. Route 컴포넌트로 특정 경로 매핑

#### 2-4. Link 컴포넌트를 사용하여 다른 페이지로 이동하는 링크 만들기

### 3. Path variable와 Query String&#x20;

#### 3-1. Path variable(URL parameter)

#### 3-2. Query String

### 4. Nested route

#### 4-1. 중첩 라우트 사용 예

#### 4-2. index props

### 5. react-router 부가 기능

#### 5-1. useNavigate

#### 5-2. NavLink 컴포넌트

#### 5-3. NotFound 페이지 만들기

#### 5-4. Navigate 컴포넌트

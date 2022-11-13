# Router

react-router를 이용한 SPA(Single Page Application) 만들기

* [react-router](https://reactrouter.com/en/main/start/overview)
  * react 내장이 아닌 third-party library로 client side routing을 지원
* SPA
  * 나오게 된 배경: MPA로 페이지 마다 서버에서 렌더링하는 건 서버 자원이 사용되는것이고, 불필요한 트래픽 낭비
  * React, Vue.js를 사용해서 뷰 렌더링을 클라이언트가 담당하고, App 구동에 필요한 자원(css, js, html, image, fonts..)를 브라우저에 로드 한 다음에 정말 필요한 데이터만 JSON으로 전달받아 렌더링
  * Router 필요한 이유:  SPA는 index.html 파일 하나만 있는데, 앱은 여러 화면이 있어야 한다. 예를 들어 블로그를 만든다면, 홈, 포스트 목록, 포스트 상세 페이지, 포스트 수정 페이지 등의 화면이 필요하다. 또한 이 화면에 접근할 수 있는 주소가 필요하다. 주소가 있어야, 유저들이 북마크도 할 수 있고 서비스에 구글을 통해 유입(SEO)될 수 있기 때문이죠. 다른 주소에 따라 다른 화면을 보여주는것을 라우팅!&#x20;
    * 리액트 자체에는 이 기능이 내장되어 있지 않아서, 우리가 직접 브라우저의 API 를 사용하고 상태를 설정하여 다른 뷰를 보여주어야 합니다. => 편하게 react-router 사용하자
  * Cons
    * public/index.html 보면 id=root 인 element만 존재하고, JS 다운 받은 뒤에 그려주는데, 기능이 많을 수록 앱 규모가 커질 수록 번들링된 JS 파일들 사이즈가 커지게 된다. => Code Spliting을 통해서 트래픽과 로딩 속도 개선, isomorphic JS(SSR + CSR) 혼용해서 사용

> 예전에 FE framework 사용 안하고 SPA 구현시에는 location의 hash(anchor) 값에 대한 change handler를 통해서 화면을 그려줬다.&#x20;

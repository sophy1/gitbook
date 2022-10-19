---
description: CSR SSR 차이(서비스, 프로젝트, 콘텐츠 성격에 따른 렌더링 방식 선택 기준)
---

# CSR vs SSR

## CSR(Client Side Rendering)

* React, Angular, Vue 는 서버 대신 브라우저에서 JS 사용하여 페이지를 직접 렌더링
* 사용자 요청에 따라 필요한 부분만 응답받아 렌더링하는 방식

### 단점

* 용량이 큰 JS 번들을 사용하면 LCP(Largest Contentful Paint)에 영향을 준다.
* 모든 JS 다운 받고 실행 완료되기 전까지는 어떤 contents 볼 수 없거나 동작을 하게 할 수 없다.

### 장점

* 서버 부하 감소
* 처음에 js 파일을 다운 받아야하기 떄문에 초기 로딩 느리지만 이후에는 빠른 렌더링 속도
* SPA(Single Page Application): 페이지 안에 contents 변경이 있어도, flickering 이슈가 없이 부드러운 이동 사용자 경험을 전



## SSR(Server Side Rendering)

* 서버에서 data가 다 채워진 HTML을 받아와서 페이지 전체를 렌더링하는 방식

### 장점 -> 선택 기준

* SEO(Search Engine Optimization) => 검색 결과 상위 노출이 필요하다.
  * 검색 엔진이 웹을 크롤링하면서 페이지에 컨텐츠 색인을 생성하는 과정
    * https://velog.io/@yena1025/SEO-robots.txt%EC%99%80-sitemap.xml%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC
    * sitemap.xml 파일 자체는 SEO 점수에 영향이 없네..?
  * SSR 채택하는 MPA(Multi Page Application)은 각각의 페이지가 있기 때문에 SEO 유리함
* 누구에게나 동일한 내용을 노출(static page)
* 빠른 초기 로딩
* SNS에서 링크를 공유했을 때 해당 웹 사이트의 정보를 이미지와 설명으로 표시해주는 OG(Open Graph) Tag를 페이지 별로 적용

### 단점

* 초기 로딩 속도가 빨라서 TTV(Time To View)와 TTI(Time To Interact)간에 시간차가 생겨, 사용자 event에 대한 핸들링이 없을 수 있음
* 매번 페이지를 요청할때마다 새로고침되기 때문에 flickering 이슈가 있어 사용자 경험이 떨어짐
* 페이지를 요청할때마다 모든 resource를 준비해서 응답하므로 서버 부하 증가
* Node.js 웹앱 실행 방법, 서버쪽 환경 구성, 클라이언트, 서버 빌드 이해가 필요
* Vue SPA의 라이프사이클 훅과는 다른 환경(브라우저가 아닌 Node.js)에서 동작하기 때문에 beforeCreated, created에서 window와 document와 같은 브라우저 객체에 접근 불가능

## SSR + CSR

* Node.js express template engine인 EJS로 빌드 타임에 static page(index page, documentation, header, footer 등 변경이 거의 없는 화면)을 생성하고,\
  로그인 이후에 화면은 CSR로 리소스를 하나로 번들링해서 구현
  * webpack.config.js에서 entry point를 여러 개로 나눔
  * router에서 static page path로 진입했을때 관련 layout 정보(title, EJS file path, description)을 가지고 res.render(  EJS FILE NAME , options) 호출



#### Ref.

* [https://nextjs.org/docs/basic-features/pages](https://nextjs.org/docs/basic-features/pages)
* [https://www.twinword.co.kr/blog/basic-technical-seo/](https://www.twinword.co.kr/blog/basic-technical-seo/)
* [https://velog.io/@hsecode/최적화-검색엔진-최적화SEO-이유와-방법-10가지](https://velog.io/@hsecode/%EC%B5%9C%EC%A0%81%ED%99%94-%EA%B2%80%EC%83%89%EC%97%94%EC%A7%84-%EC%B5%9C%EC%A0%81%ED%99%94SEO-%EC%9D%B4%EC%9C%A0%EC%99%80-%EB%B0%A9%EB%B2%95-10%EA%B0%80%EC%A7%80)

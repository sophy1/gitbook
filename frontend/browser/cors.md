---
description: 교차 출처 리소스 공유(Cross-Origin Resource Sharing, CORS)
---

# CORS

보안 상의 이유로, 브라우저는 스크립트에서 시작한 교차 출처 HTTP 요청을 제한한다. 따라서 다른 origin에 있는 resource를 API를 통해 가지고 와서 사용할때 SOP(Same Origin Policy)에 의해서 사용할 수 없다. 이를 해결하기 위한 방법이 CORS이고, 자세한 내용은 아래와 같다.

### SOP(Same Origin Policy)

#### 기본 개념

<figure><img src="../../.gitbook/assets/image (7) (1).png" alt=""><figcaption><p>출처: <a href="https://blogpack.tistory.com/m/105">https://blogpack.tistory.com/m/105</a></p></figcaption></figure>

* Origin
  * URL에서 protocol(scheme), host(domain), port 를 합친 부분
  * console.log(location.origin) 으로 알 수 있음
  * 예: https://velog.io/series?sort=asc#top
    * Procotol: https(://)
    * Host: velog.com
    * Port: 생략되어 있지만 (:80, :443)
    * Path: series
    * Query Parameter: ?sort=asc
    * Anchor(Fragement): #top
* 같은 출처
  * https://user:password@velog.io
  * https://velog.io/about
* 다른 출처
  * https://api.velog.io
  * http://velog.io/series
*   Resource(리소스, 자원)

    * URI(자원 식별자): URL(자원의 위치), URN(자원의 이름), URC, Data URI



### SOP, CORS는 무엇이고 왜 필요한가?

* SOP: 같은 origin에서만 리소스를 공유할 수 있다는 규칙을 가진 정책
* CORS: 다른 origin에 있는 리소스를 가지고 와서 사용할 수 있도록 예외 조항을 둔 정책
* 다른 origin의 리소스를 요청한다는 SOP 정책을 위반한 것이고, SOP 예외 조항인 CORS 정책까지 지키지 않는다면 다른 출처의 리소스를 사용할 수 없음.
* CORS 정책 위반시 로컬에서 프론트엔드 개발시 분리된 백엔드 서버의 API 호출시 콘솔에서 아래와 같은 에러를 확인할 수 있음

```
Access to fetch at ‘https://dev.A.com/’ from origin ‘http://localhost:8080’ has been blocked by CORS policy: No ‘Access-Control-Allow-Origin’ header is present on the requested resource. If an opaque response serves your needs, set the request’s mode to ‘no-cors’ to fetch the resource with CORS disabled.
```

* 자유롭게 다른 출처의 API 사용하고 싶은데 CORS 왜 지켜야 하나?
  * 브라우저, 클라이언트 앱은 세션, 쿠키 등 사용자 정보가 담겨져 있고 이에 대한 XSS(Cross-Site Scripting), CSRF(Cross-Site Request Forgery)와 같은 보안 공격에 탈취당하기 쉽기 때문에

### CORS는 어떻게 서로 다른 출처를 가진 리소스를 안전하게 사용할 수 있게 하나?

* Simple Request
  * GET, POST 방식 요청인 경우, 브라우저가 서버로 API request를 보낼때 header에 origin 설정해서 보냄
  * 서버는 response header에 Access-Control-Allow-Origin에 서버에 등록된 URL정보를 담아준다.
  * 브라우저가 CORS 정책 위반 여부를 검사하여 origin에 있는 값이 ACAO에도 담겨 있다면 data를 주고 받을 수 있음
* Preflight Request
  * Simple request와 동일하지만 그 전에 브라우저가 OPTIONS 메소드를 사용하여 해당 request의 권한이 있는지 여부를 확인하기 위해서 미리 request를 보낸다.
* Credentialed Request

### CORS 적용할 수 있는 방법

* Access-Control-Allow-Origin(이하 ACAO) 설정
  * Spring, Express, Django 등 대부분 BE 프레임워크에서는 ACAO 헤더에 알맞는 값을 설정할 수 있는 방법을 제공한다.
*   Webpack Dev Server로 reverse proxying

    * BE에는 이미 ACAO 헤더가 세팅되어 있지만, 이 헤더에다가 FE 개발 서버 'http://localhost:8080'과 같은 범용적인 출처를 넣어주는 경우는 없다고 본.
    * 로컬에서 동일한 domain을 가지고, BE API 개발 서버로 API request를 보낼때 webpack, webpack-dev-server에서 일부 URL을 프록시하여 CORS 정책을 우회할 수 있다.
      * vue.config.js, webpack-dev-server 설정

    ```
    devServer: {
        proxy: {
          '/api': {
            target: process.env.API_URL,
            changeOrigin: true,
            pathRewrite: {"^/api": "/agent"}, // /api로 시작하는 문자를 /agent 로 변경
          },
    }
    ```
* Vanilla JS로 개발할때는 module 스크립트를 실행하거나 외부 API 사용하려면, VSCode extension인 live server 설치해서 proxy 설정을 해야 한다.&#x20;

### Ref.

* [https://developer.mozilla.org/ko/docs/Web/HTTP/CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)
* [https://webpack.kr/configuration/dev-server/#devserverproxy](https://webpack.kr/configuration/dev-server/#devserverproxy)

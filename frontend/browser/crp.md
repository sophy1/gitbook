---
description: 브라우저 동작 원리와 중요 렌더링 경로(Critical Rendering Path, CRP)
---

# 동작 원리와 CRP

### 브라우저에 대해 알아보자

{% tabs %}
{% tab title="브라우저 아키텍처" %}
#### 브라우저 구성 요소

1. 사용자 인터페이스 - 주소 표시줄, 이전/다음 버튼, 북마크 메뉴 등. 요청한 페이지를 보여주는 창(View) 을 제외한 나머지 모든 부분이다.
2. 브라우저 엔진 - 사용자 인터페이스와 렌더링 엔진 사이의 동작을 제어.
3. 렌더링 엔진 - 요청한 콘텐츠를 표시. 예를 들어 HTML을 요청하면 HTML과 CSS를 파싱하여 화면에 표시함.
4. 통신(Networking) - HTTP 요청과 같은 네트워크 호출에 사용됨. 이것은 플랫폼 독립적인 인터페이스이고 각 플랫폼 하부에서 실행됨.
5. UI 백엔드(Display BE) - 콤보 박스와 창 같은 기본적인 widget을 그림. 플랫폼에서 명시하지 않은 일반적인 인터페이스로서, OS 사용자 인터페이스 체계를 사용.\
   ![](<../../.gitbook/assets/image (3).png>)
6. 자바스크립트 해석기 - 자바스크립트 코드를 해석하고 실행.
7. 자료 저장소(Data Persistence) - 이 부분은 자료를 저장하는 계층이다. 쿠키를 저장하는 것과 같이 모든 종류의 자원을 하드 디스크에 저장할 필요가 있다. HTML5 명세에는 브라우저가 지원하는 Web storage 정의되어 있다.



<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>출처: <a href="https://www.semanticscholar.org/paper/CISC-322-Fall-2009-Assignment-1-Conceptual-of-Burstyn-Cole/8043762a3263e04f00e38c597b126e70881d9f6b">https://www.semanticscholar.org/paper/CISC-322-Fall-2009-Assignment-1-Conceptual-of-Burstyn-Cole/8043762a3263e04f00e38c597b126e70881d9f6b</a></p></figcaption></figure>
{% endtab %}

{% tab title="Rendering Engine" %}
| 브라우저    | 렌더링 엔진                  | JS Interpreter |
| ------- | ----------------------- | -------------- |
| IE      | Trident                 |                |
| Edge    | EdgeHTML, Blink         |                |
| Chrome  | Webkit, Blink(버전 28 이후) | V8             |
| Safari  | Webkit                  |                |
| FireFox | Gecko                   | Spider Monkey  |
{% endtab %}

{% tab title="Rendering 과정" %}
아래는 중요 렌더링 경로를 요약한 내용이다.

1. 브라우저는 HTML, CSS, JS, 이미지, 폰트 파일 등 렌더링에 필요한 리소스를 request하고 서버로부터 response를 받는다.
2. 브라우저의 렌더링 엔진은 서버로부터 응답된 HTML과 CSS를 파싱하여 DOM tree과 CSSOM tree을 생성하고 이들을 결합(Attachement)하여 렌더 트리를 생성한다.
3. 브라우저의 JS Engine(interpreter)은 서버로부터 응답된 자바스크립트를 파싱하여 AST를 생성하고 바이트코드로 변환하여 실행한다. 이때 자바스크립트는 DOM API를 통해 DOM이나 CSSOM을 변경할 수 있다. 변경된 DOM과 CSSOM은 다시 렌더 트리로 결합된다.
4. 렌더 트리를 기반으로 HTML 요소의 layout(위치와 크기)을 계산하고 브라우저 화면에 HTML 요소를 painting, composition(GPU가 layer를 합치는 과정) 한다.



<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption><p>출처: <a href="https://poiemaweb.com/js-browser">https://poiemaweb.com/js-browser</a></p></figcaption></figure>
{% endtab %}

{% tab title=" Networking" %}
브라우저 모듈과 관련이 없지만, HTTP request 발생시 네트워크에 대한 간단한 이&#x20;

1. DNS를 통해 서버 IP 주소를 얻는다.
   1. 먼저 브라우저의 DNS cache에서 찾는다.
   2. 없으면 OS cache (hosts 파일)에서 찾는다.
   3. Router cache, ISP(Internet Service Provider) cache 에서 찾는다.
   4. DNS query 를 날려서 recursor가 root name server -> top-level domains(.com) -> second-level domains(google.com) -> third-lever domains(www.google.com) 재귀적으로 찾아서 반환한다.
   5. 이때 Routing table을 통해 서버까지 어떤 경로로 갈지 결정하고, IP 주소 획
2. TCP/IP
   * 3 way handshaking
3. HTTP request
   * Header
   * Method
   * 쿠
4. 서버에서 HTTP response&#x20;
   * 상태 코드
   * 캐시 방법(Cache-Control)
   * 압축 유형(Content-Encoding)
   * 쿠키
   * Contents
     * JSON, XML, HTML
5. 리소스 다운로드
{% endtab %}
{% endtabs %}

위 렌더링 과정을 좀 더 자세히 살펴보자....

### 중요 렌더링 경로(Critical Rendering Path, CRP)

1.

###

###

### Ref

* [https://develope.mozilla.org/ko/docs/Web//Critical\_rendering\_path](https://developer.mozilla.org/ko/docs/Web/Performance/Critical\_rendering\_path)
* [https://d2.naver.com/helloworld/59361](https://d2.naver.com/helloworld/59361)
* [https://chanho-yoon.github.io/web/web-render/](https://chanho-yoon.github.io/web/web-render/)

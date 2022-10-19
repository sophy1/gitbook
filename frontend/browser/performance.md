---
description: Web 성능 중 FE와 관련있는 부분만 다루어 봅니다.
---

# Performance

### Web FE 성능

인터넷 속도, CDN 사용 여부, 서버 위치, 접속자 수, 웹사이트 최적화 정도, Cache-control에 좌우된다.

서비스 지연 속도가 늘어날 수록 사용자 이탈률이 급격하게 증가한다. 성능 최적화를 통해 사용성을 개선하고 구매 전환률과 이익 증대를 이끌어낸다.

(개인적으로도 개발할때 3초 안에는 원하는 정보가 표시되야 답답하지 않다. 인프라나 BE 문제로 테스트도 제대로 안 된다? 휴...인내심 향상 시간으로 삼아야 함)&#x20;

### 성능 지표 및 측정 방법

#### 성능 지표&#x20;

1. 로딩 속도
   * FCP(First Contentful Paint): 첫 요소가 로드될 때까지 걸리는 시간
   * FMP(First Meaningful Paint): 의미있는 첫 요소가 로드될 때까지 걸리는 시간
   * LCP(Largest Contentful Paint): 초기 DOM 콘텐츠가 로드될 때까지 걸리는 시간
   * 위 metric 중 LCP 기준으로 로딩 속도 측정하며, 임계값 2.5초 \~ 4.0초 미만이면 개선 필요, 4초 이상이면 production으로 내보내면 안 됨
2. 렌더링 시간
   * 브라우저 렌더링 과정(Critical Rendering Path)에 걸리는 시간
   * 초당 60 프레임(사람이 자연스럽다고 느껴지는 초당 화면 수)의 속도로 reflow 및 repint가 발생할 수 있도록 보장
   * 렌더링 과정을 이해하고 최적화하는 것은 사용자 상호작용을 원활하게 하고, 버벅거림을 방지할 수 있음
3. 메모리 누수
   * 메모리에 할당된 자원이 제때 해제되지 않고, 계속 메모리에 남아 있는 현상
   * 대부분 GC에 의해서 해제되지만, 아래와 같은 이유로 해제되지 않는 경우가 있음
     * [전역 변수, 해제되지 않은 타이머/콜백, DOM 외부에서의 참조, 클로저 ](https://ui.toast.com/posts/ko\_20210611)
4. [Web Vitals](https://web.dev/i18n/ko/vitals/)
   * Google에서 제시하는 필수 성능 지표
   * LCP, FID, CLS
     * FID(First Input Delay, 최초 입력 지연): _상호작용 측정_. 사용자 행동에 대해 이벤트 핸들러가 반응하기까지 걸리는 시간. 100ms 이하면 좋음.&#x20;
     * CLS(Cumulative Layout Shift, 누적 레이아웃 시프트): _시각적 안정성 측정_. 시작 위치에서 레아이웃이 얼마나 변화하는지에 대해 측정. 0.1 미만이 좋음&#x20;

#### 성능 측정 도구

* Ligthhouse: Chrome 개발자 도구에서 제공
  * Web Vitals 이용하여 성능 측정하고 결과 reporting
* 브라우저 개발자 도구&#x20;
  * Performance 탭: 원하는 구간에 녹화해서 network, rendering, memory 전반에 대한 정보 제공
  * Memory 탭: 현재 메모리 사용 확인. 스냅샷 차이. 누수 발생 항목을 찾을 수 있음.
  * Network 탭: 요청 처리 시간 확인. 모바일 환경과 유사한 네트워크 설정으로 시뮬레이션 가능
* Vue devtools, React 프로파일러
  * 컴포넌트 렌더링 시간 파악
  * 사용자 활동(interaction)에 대한 변화 추적
* 모니터링 도구
  * 실시간 서비스 중에 성능 측정
  * [제니퍼 프론트](https://front.jennifersoft.com/docs/ko/#/), [뉴렐릭](https://newrelic.com/kr)

#### 성능 측정 방법

* 현재 상황과 제공할 서비스 성격에 맞게 개선하고하는 요소를 정하는 것이 좋다.
* 기본 환경에서 측정해야 한다.
  * 확장 프로그램과 같이 성능에 영향을 미치는 요소 제거
  * 시크릿모드 사용 (로컬 캐시 없이)
* 타켓 사용자 환경에 맞춰서 데이터 수집해야 한다.
  * 모바일 환경이라면 웹 환경이 아니라 기기를 이용해서 성능 측정

### 성능 개선을 위한 최적화 방법

사용자 입장에서는 빠르게 컨텐츠를 확인하고, 상호 작용시에 버벅거림 없이 매끄러운 경험을 원한다.

* 로딩, 렌더링 최적화: 리소스(HTML, JS, 이미지, 폰트 등)을 빠르게 로드해서, 화면에 그려줘야 한다.
* UX 최적화: Reflow(layout 변경), Repaint 과정을 최적화하여, 스크롤링, 브라우저 크기 변경, 키보드 입력 이벤트가 발생시에 DOM 스타일 계산을 최소화 한다. &#x20;

> 이중 tab화면(5개 tab menu 하위에 10개의 tab menu)을 개발할 일이 있었는데,\
> 전임자가 table 데이터를 가져오는 script tag를 tab마다 작성해두었다.\
> script 로드하느라 렌더링 blocking이 너무 심해서 body tag 하단에 넣고  defer attribute 추가해서 간단히 해결한적이 있다.

### 로딩, 렌더링 최적화 방법&#x20;

#### [Virtual Dom](https://ui.toast.com/weekly-pick/ko\_20210819)

#### HTML에서 JS 불러오는 방법

브라우저는 HTML 파싱하고 DOM트리를 생성하는데, HTML 태그를 읽어나가는 도중 태그를 만나면 파싱을 중단하고, JS 파일을 로드하고 JS 코드를 파싱한다.

#### body 최하단 (가장 일반적이다)

* HTML 파싱 중에 브라우저 화면에서 script 태그로 인한 중단/지연 시간이 발생한다.
* DOM 트리가 생성되기 전에 JS가 생성되지도 않는 DOM 조작을 시작할 수도 있다. 위와 같은 상황을 막기 위해서 body 태그 최하단에 위치
* 우선 서버로부터 HTML문자열을 STREAM 으로 받게된다. 그러고 태그에 포함된 자원을 병렬로 다운로드 한다. 어떻게 보면 속도가 향상된다 생각할 수 있지만, 이작업을 할동안 페이지에는 아무것도 로드 되지않기때문에, 보통 여기다가 js를 포함시키지 않는다. (이유는 js파일들이 보통 크기 때문에 또한 나중에 dom을 바꿀 수 있는 문법이 있을수있어서) 고로 head 태그에는 css와 필수 js만 넣어야 되고, js는 바디태그 마지막에 넣는게 통상적으로 가장 효율적인 방법이다. \
  defer(돔제어 관련있는 스크립트)혹은 async(의존성이 없는 스크립트)를 이용해서 javascript를 다운로드 받을 수 있다. preload를 통해서 폰트나 이미지를 같이 받는 방법이 있을 수 있다.&#x20;

#### head 마지막 부분

* body 코드 불러오기 전에 JS 다운 받고 실행한 뒤에 HTML 코드를 받아옴
* JS 크기가 큰 경우, 전체 웹 페이지를 보여주는데 시간이 오래 걸릴 수 있음

#### body 끝 부분

* HTML 먼저 보여주고 JS 나중에 다운받고 실행하기 때문에 만약 JS 화면에 필요한 여러 중요한 동작들을 수행한다면
* 개발자가 의도한 페이지의 동작을 다 보여주기에는 여전히 느리다

#### async, defer 속성을 활용하면 스크립트 로딩 순서를 제어할 수 있다.

#### async

* HTML 파싱할때 JS fetching도 병렬적으로 실행
* async 로 여러 JS 불러온다고 하면, fetching이 먼저 끝나는 순서대로 HTML parsing 끝나기 전에 JS 실행되기 때문에 개발자가 작성한 순서대로 프로그램이 동작하지 않을 수 있음

#### defer

* HTML 파싱할때 JS fetching 병렬적으로 동작, HTML parsing이 끝나고 개발자가 작성한 순서대로 실행

#### script 내부에서 로딩 순서 제어하기 DOMContentLoaded, onload를 활용하면 JS 내에서 로딩 순서 제어 가능

* DOM 생성 이후에 DOMContentLoaded 실행
* DOM, images, script, css 모든 contents 로드된 후에 onload 실행

#### Rendering 성능 개선

* DOM tree (페이지 구조를 표현) + css 모델 => 렌더 트리(DOM 노드를 어떻게 표시할지를 나타냄)
* Reflow (많은 비용이 드는 작업) => 변경 사항을 큐에 모았다가, 한 번에 처리하는 방식으로 최적화
  * 보이는 DOM element를 추가하거나 제거했을대
  * 요소의 위치가 바뀌었을때
  * 요소의 크기(마진, 패딩, border 두께, 너비, 높이) 바뀌었을때
  * 텍스트 내용이 바귀꺼나 이미지가 다른 크기의 이미지로 대체되는 등 내용이 바뀌었을때
  * 페이지를 처음 표시
  * 브라우저 창의 크기를 바꾸었을때(resize)
* Repaint
* CSS, DOM 스크립팅 최적화

### Ref.

* [LCP(Largest Contentful Paint) 최적화하기](https://ui.toast.com/posts/ko\_202012101720)
* [https://velog.io/@jgam/FE-성능분석에-대한-고찰](https://velog.io/@jgam/FE-%EC%84%B1%EB%8A%A5%EB%B6%84%EC%84%9D%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0)
* [https://velog.io/@bumsu0211/브라우저-렌더링-과정과-최적화](https://velog.io/@bumsu0211/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95%EA%B3%BC-%EC%B5%9C%EC%A0%81%ED%99%94)

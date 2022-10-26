# ES6 modules

### Module

개발하는 app 규모가 커질 수록 공통 코드는 분리된 파일(모듈)로 만들어서 사용한다. 모듈은 대개 클래스 하나 또는 특정한 목적을 가진 복수의 객체로 구성된 라이브러리 하나로 볼 수 있다.

App을 실행하는 환경은 Node.js, 브라우저 등 다양한 런타임 환경을 가질 수 있고, 비동기적인 상황과 브라우저간 호환성을 해결하기 위해 다양한 모듈 시스템이 생겨나게 됐다.&#x20;

여기서는 ES6 모듈에 대해서만 알아보고, 직접 사용 예를 살펴보기로 한다.

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>출처: <a href="https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/">https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/</a></p></figcaption></figure>

### 여러 객체/값을 내보내고 불러오는  방법(Named exports)

#### 먼저 \`export\`키워드로 내보기! <a href="#javascriptes6module-export" id="javascriptes6module-export"></a>

<pre class="language-javascript"><code class="lang-javascript">// functions, var, let, const, class를 내보낼 수 있지만, 최상위 항목이어야 한다.
// 예를 들어, 함수 안에서 export를 사용할 수 없다.

<strong>export const name = 'square';
</strong>
// 안 내보냄
function applyScale(len, scale = 1) {
  return len * scale;
}

export function draw(ctx, length, x, y, color) {
  ctx.fillStyle = color;
  ctx.fillRect(x, y, applyScale(length), (length));

  return {
    length: length,
    x: x,
    y: y,
    color: color
  };
}

const drawDoubleScale = function () { ... };

export { drawDoubleScale };</code></pre>

* 여러 항목을 내보낼때 모듈 파일 끝에 하나의 export 문을 사용하는 것이 일반적이다.

```javascript
export { name, draw, reportArea, reportPerimeter };
```

```javascript
// index.js
// 하위 UI component에서 export default 로 내보낸 객체를 받아와서 다시 export
export { default as Grid } from 'Grid';
export { default as Table } from 'Table';
export { default as DatePicker } from 'DatePicker';
```

#### import 키워드로 가져오기!

<pre class="language-javascript"><code class="lang-javascript"><strong>import { name, draw, reportArea, reportPerimeter } from './modules/square.js';
</strong>
// 모듈에서 하나의 멤버만 가져옵니다. 
<strong>import { drawDoubleScale } from './modules/squre.js';
</strong>import { reallyReallyLongModuleMemberName as shortName} from "my-module.js";

// 다른 라이브러리를 모두 가져오는 것보다, bundle 사이즈를 줄이기 위해서 필요한 멤버만 가져온다.
// import _ from 'lodash';
import { map, tail, times, uniq } from 'lodash';
// github.com/facebook/react/blob/main/packages/react/index.js
import React, { useRef, useEffect } from 'react';

// export 에도 alias를 사용할 수 있지만, 가급적 import할때 사용한다.
import { name as squareName,
         draw as drawSquare,
         reportArea as reportSquareArea,
         reportPerimeter as reportSquarePerimeter } from './modules/square.js';

import { name as circleName,
         draw as drawCircle,
         reportArea as reportCircleArea,
         reportPerimeter as reportCirclePerimeter } from './modules/circle.js';
         
// 모듈 전체로 가져와서 (alias 별명)에 바인딩하여 사용한다.
import * as CONSTANTS from './constants.js';
import * as Square from './modules/square.js';
let square1 = Square.draw(myCanvas.ctx, 50, 50, 100, 'blue');
Square.reportArea(square1.length, reportList);
Square.reportPerimeter(square1.length, reportList);</code></pre>

* 일부 모듈 시스템에서는 파일 확장명을 생략할 수 있다. 예를 들어, react에서는 컴포넌트를 import할때 js를 생략한다.

```javascript
import Hello from "@/Hello";
import Wrapper from "@/Wrapper";
```

### 하나의 객체/값을 내보내고 불러오는  방법(Export default)

#### \`export default\` 를 추가하여, 내보내기!

<pre class="language-javascript"><code class="lang-javascript"><strong>// 중괄호 없음!
</strong><strong>export default &#x3C;함수 이름, 익명 함수, 객체 이름, 변수 이름 등>;
</strong><strong>
</strong>export default axiosInstance;

export default {
    ...USER_FORM,
    ...PAYMENT_FORM,
};

// TooltipBox.js
class TooltipBox {};
export default TooltipBox;
</code></pre>

#### \`import\`로 namedExports랑 동일하게 가져오기!

```javascript
import { default as axiosInstance } from './axios-util.js';
// 위 코드를 단축하면 아래와 같다.
import axiosInstance from './axios-util.js';

// 일반적으로 아래와 같이 가져옴.
import TooltipBox from './components/TooltipBox';
```

### Module 모으기

여러 서브 모듈을 하나의 부모 모듈로 결합하여, 이 상위 모듈을 내보낼 때 사용한다.

```
main.js
modules/
-- canvas.js
-- shapes.js
-- shapes/
---- circle.js
---- square.js
---- triangle.js
```

```javascript
// square.js
export { Square };

// shapes.js (집합 aggregation 부분)
export { Square } from './shapes/square.js';
export { Triangle } from './shapes/triangle.js';
export { Circle } from './shapes/circle.js'

// main.js 등 shapes의 세 개의 모듈 클래스를 사용할때
import { Squre, Circle, Triangle } from './modules/shapes.js';
```

### Applying the module to  HTML code

<pre class="language-javascript"><code class="lang-javascript"><strong>// module로 선언하려면 HTML code에서 script element에 type="module" 추
</strong><strong>&#x3C;script type="module" src="main.js">&#x3C;/script></strong></code></pre>

일반 script와 module 의 차이

* 로컬 테스트에서의 주의 사항 — HTML파일을 로컬(예를들어 `file://` URL)에서 로드하려고 하면, 자바스크립트 모듈 보안 요구 사항으로 인해 CORS 오류가 발생합니다. 개발 서버를 통해 테스트 해야 합니다. => 웹팩 설치해서 dev-server 실행해서 해결
* 표준 스크립트와 달리 모듈 내부에서 정의된 스크립트 섹션과는 다르게 동작할 수 있습니다. 이는 모듈이 자동적으로 [strict mode](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Strict\_mode)를 사용하기 때문입니다. => this는 undefined&#x20;
* 모듈 스크립트를 불러올 때 `defer` 속성([`<script>` attributes](https://developer.mozilla.org/ko/docs/Web/HTML/Element/script#attributes))를 사용할 필요가 없습니다. 모듈은 자동으로 defer됩니다. => HTML parsing 중단 없이, 모두 parsing이 끝난 뒤에 script 순서대로 실행, 병렬적으로 외부 모듈 스크립트와 리소스 로드, 스크립트 작성 순서 대로 실행 순 유지됨
* 마지막으로 모듈 기능을 단일 스크립트의 스코프로 가져왔음을 분명히 해야 합니다. — 전역 스코프에서는 사용할 수 없습니다. 따라서 import한 스크립트에서 가져온 기능에만 접근할 수 있습니다. 예를들어 자바스크립트 콘솔에서 접근할 수 없습니다.

### Ref

* https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Modules
* https://wormwlrm.github.io/2020/08/12/History-of-JavaScript-Modules-and-Bundlers.html
* ESM 을 이용한 빌드 툴 Vite: [https://vitejs-kr.github.io/guide/why.html#the-problems](https://vitejs-kr.github.io/guide/why.html#the-problems)
* https://medium.com/dailyjs/javascript-module-cheatsheet-7bd474f1d829


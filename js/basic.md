---
description: JavaScript 기본 개념
---

# JS Basic

### 프로토타입, 프로토타입 체이닝, Class, 상속, 생성자

https://developer.mozilla.org/ko/docs/Web/JavaScript/Inheritance\_and\_the\_prototype\_chain

* 각각의 객체는 \[\[Prototype]]이라는 은닉(private) 속성을 가지는데 자신의 프로토타입이 되는 다른 객체를 가리키고 있다.
* \[\[prototype]] 프로토타입 객체에 대한 링크를 가진다. 객체의 어떤 속성에 접근하려할 때 그 객체 자체 속성(property) 뿐만 아니라 객체의 프로토타입, 그 프로토타입의 프로토타입 등 프로토타입 체인의 종단(null)에 이를 때까지 그 속성을 탐색한다.
* 프로토타입 객체는 상위 프로토타입 객체로부터 메소드, 속성을 상속 받을 수 있다. 연속적으로 상속할 수 있는 것을 프로토타입 체인이라고 하며, 다른 객체에 정의된 메소드, 속성을 prototype (private 속성)을 통해 사용할 수 있게 한다.
* 객체 생성자: (일반적으로 함수 이름을 대문자로 시작함) 일반 함수와 new를 통해서 새로운 객체를 생성할 수 있다. 상속받은 객체 생성자 함수를 만들고 나서, prototype 값을 Animal.prototype으로 설정.
* 클래스(ES6): class 키워드를 사용해서 객체 생성자로 구현했던 코드를 깔끔하게 구현할 수 있게 해 주고, extends키워드를 사용하여 상속도 쉽게 해줄 수 있다.
  * extends할때, super() 함수는 상속받은 클래스의 생성자를 가리킨다.
  * 클래스에서는 prototype에 부모 객체 프로토타입을 복사? 설정하지 않아도 된다.

### Scope, scope chaining

* Scope: 변수, 함수가 선언될때 사용 가능한 범위
  * Global (전역): 코드의 모든 범위
  * Function (함수): 함수 안에서만 사용 가능
  * Block (블록): if, for, switch, module? 등 특정 블록 내부에서만 사용 가능
    * 블록 밖의 범위에서 똑같은 이름을 가진 변수가 있다해도 영향을 주지 않는다.
    * 호이스팅과 연관지어 생각!

### 실행 context

context stack에 전역실행 context를 생성. 함수 호출시에 함수 실행 context가 쌓임; VO(변수, 함수선언, 매개변수, 인수argument), Scope chain, this에 대한 정보가 담김

### this !

https://poiemaweb.com/js-this

* 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 this에 바인딩할 객체가 동적으로 결정( != 렉시컬 스코프)
* 함수 호출 방식
  1. 일반 함수 호출(전역 scope)에서 this는 window가 바인딩.
     * 내부함수는 일반 함수, 메소드, 콜백함수 어디에서 선언되었든 관계없이 this는 전역객체를 바인딩
  2. 객체의 메서드 호출에서 this는 메서드를 호출한 객체에 바인딩.
  3. new와 함게 생성자 함수 호출에서 this는 생성될 instance를 가르킨다.
  4. this를 명시적으로 바인딩할 수 있는 Function.prototype.apply, call, bind 메소드를 제공
* call, apply, bind
  * 모두 함수를 호출
  * call, apply는 함수 호출에 필요한 파라미터를 전달하는 방식이 다름. .call은 쉼표로 구분된 값들, .apply는 배열.
  * bind는 인자로 전달한 this가 바인딩된 새로운 함수를 리턴한다. 즉, Function.prototype.bind는 .apply, .call 메소드와 같이 함수를 실행하지 않기 때문에 명시적으로 함수를 호출할 필요가 있다.

### ES6 추가 기능

1. 메서드 함수 (일반함수, 생성자 함수와 비교할 수 있어야함)
2. 화살표 함수
3. Promise, Async, Await
4. rest 파라미터
5. Destructuring assignment(구조 분해 할당): 객체, 배열의 값을 추출해서 변수에 바로 할당

### 화살표 함수 (arrow function expression)

https://poiemaweb.com/es6-arrow-function

* function keyword 없이 =>를 이용해 함수를 생성. return 명령어 없이도 함수 실행을 종료시키고 값을 반환한다. 예를 들어, const sum = function (a, b) { return a + b; } 화살표 함수 const sum = 매개변수 (a, b) => a + b; 실행문의 결과가 바로 함수의 반환값
* https://velog.io/@grinding\_hannah/JavaScript-%ED%99%94%EC%82%B4%ED%91%9C%ED%95%A8%EC%88%98arrow-function-%EC%95%8C%EA%B8%B0
* function 키워드 사용하는 것보다 짧고, map, reduce, sort 과 같은 함수의 콜백함수로 사용되기 쉬움
* 항상 익명(재사용하지 않을 목적으로 함수에 이름을 붙이지 않는 함수) 이기 때문에 생성자로서 사용할 수 없다.
* this/arguments/super/prototype/new.target 바인딩하지 않는다.
* 일반 함수는 this가 어떻게 호출되었는지에 따라 바인딩할 객체가 (동적으로) 결정되는데, 화살표함수의 this는 화살표함수가 호출되는 시점과는 무관하게 선언되는 시점에 결정되며 언제나 상위 스코프의 this를 가리킨다.
* 렉시컬 스코프(Lexical Scope): 함수를 어디서 선언하였는지에 따라 상위 스코프가 정해지는 방식.

```javascript
var obj = { 
    myName: 'Barbie', 
    logName: function() { 
        console.log(this.myName); 
    }
};

obj.logName();   //"Barbie"
//콜백함수로 일반함수표현방식을 사용하면 dot notation법칙에 따라 this는 obj객체가 된다. 

var obj = { 
    myName: 'Barbie', 
    logName: () => { 
        console.log(this.myName); 
    }
};

obj.logName();   //undefined 
/*콜백함수로 화살표함수를 사용하면 이 예제의 경우 this는 상위 스코프인 전역스코프 혹은 
window객체가 된다. 
현재 상위 스코프에는 변수 myName이 존재하지 않으므로 undefined를 반환한다.*/

window.myName = "Hannah"; 
var obj = { 
    myName: 'Barbie', 
    logName: () => { 
        console.log(this.myName); 
    }
};

obj.logName();   //"Hannah"
//여기서 this는 obj객체의 상위스코프의 this인 window객체가 된다. 
```

### Type

* Primitive type, Object type
* primitive type
  * 한 번 선언되면 memory reference 값 변경이 불가능
  * number, string, boolean, undfined, null, symbol
  * 할당시 변수에 실제 value 저장
  * 다른 변수에 할당 시 '값'이 복사되어 전달(pass by value)
    * Pass by Value, Pass by Reference 는 인수(Arguements)를 함수에 전달할때 데이터 유형에 따라 어떻게 넘겨줄지 결정
    * Call by \~ 는 함수의 입장에서, Pass by \~ 는 인수의 입장에서 설명되는 차이가 있다.
    * Pass by Value (값의 복사에 의한 전달): Pass by Value는 인자로 넘기는 값을 그대로 복사해서 함수에 전달하는 방식
  *   수정하면 새로운 주소에 값이 할당

      ```
        let str = 'hello'; // str1 값 변경시 기존 메모리 주소에 접근해 해당 value를 저장하는 것이 아니라
        str = 'world'; // 새로운 메모리 주소에 world 라는 문자열 할당, str1에 해당 메모리 주소를 할당
      ```
  * null, undefined 차이
    * undefined은 변수를 선언하고 값을 할당하지 않은 상태. let a;
    *   null은 변수를 선언하고 빈 값을 할당한 상태(빈 객체)이다.

        ```javascript
        typeof null // 'object'
        typeof undefined // 'undefined'
        null === undefined // false
        null == undefined // true
        null === null // true
        null == null // true
        !null // true
        !undefined // true

        isNaN(1 + null) // false
        isNaN(1 + undefined) // true
        ```
  *   NaN: Not-a-Number

      ```javascript
        console.log(0 / 0); // NaN
        console.log(100 / 'hi'); // NaN
        typeof NaN === 'number'
        1 + null // 1
        1 * null // 0
        1 / null // Infinity
        1 / undefined // NaN
      ```
* Object type
  * 객체, 함수, 배열 등 거의 모든 것이 객체
  * 할당 시 변수에는 메모리 주소(참조값)이 저장
  * 다른 변수 할당 시 '참조' 값이 할당되어 같은 객체를 공유
    * Pass by Reference는 인자로 넘기는 값의 메모리상의 주소 를 복사해 함수에 전달하는 방식
    * 만약 우리가 메서드에 인자로써 객체 또는 배열을 전달한다면 객체의 값을 변경할 수 있는 가능성이 생깁니다
  * 수정 가능 하기 때문에 property 동적으로 추가, 삭제 가능
* Type 형변환
  * String -> Int: parseInt()

#### Function 만드는 방법

* 일반 함수(함수 선언식)
* 함수 표현식
* 객체 메소드
* 화살표 함수
* 즉시실행함수

#### Object type 더 자세히..

#### Native object(Built-in object), Host object, User-defined object

* Built-in object가 있는 이유? Primitive type은 프로퍼티와 메서드를 가질 수 없는데 str은 어떻게 length 프로퍼티를 가지고, lowerCase 메서드를 호출 할 수 있는걸까?\
  이유는 JS 엔진이 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌려주기 때문이다. 이렇게 임시로 생성되는 객체를 래퍼 객체 라고 한다. 생성된 임시 래퍼 객체는 사용 후 가비지 컬렉터에 의해 해제된다.

#### Object 생성 방법

* Object literal
* new 생성자(contstructor)
* Object.create()
  * 첫 번째 인자로 프로토타입 객체, 두 번째 인자로 객체의 property를 넘겨줘서, 기존 객체를 상속해 확장할때 사용
  * new Object와 같이 새로운 객체를 생성하지만 생성자 호출 하지 않음
* closure (Factory function)
  * private data 저장할 수 있음
  * 메모리 관리 어려움

#### Array method

* for.. in / for ...of for ... in을 사용하면 객체 내에 있는 key들의 목록을 얻을 수 있다. 이를 통해 객체를 탐색할 수 있다. cf. for ... of는 객체 내의 value의 목록을 얻을 수 있는 구문이다. 여기서 알아둬야할 점은, 객체를 탐색할 때 그냥 for ... in을 사용하는 것 보다는 for ... in에 hasOwnProperty()를 같이 사용하는 것이 안전하다. for ... in의 경우 우리가 원하던 것처럼 해당 객체만 탐색하는 것이 아니라, 상위 객체인 prototype까지 확인하기 때문이다. 그래서 eslint airbnb의 경우, for ... in의 사용을 자제하도록 경고를 띄운다. 아래 코드와 그 결과값을 보면 더 쉽게 확인이 가능하다.
* Array.keys(array): 파라미터로 맏응 array의 index를 key 값으로 갖는 array iterator를 반한
* map
* reduce
* filter
* sort
* forEach() map() 차이점
  * 둘 다 for 대체할 수 있는 함수로 배열의 모든 요소를 순회하면서, 인수로 전달받은 콜백함수를 반복 호출. 원본 배열을 변경하지 않음.
  *   forEach 반환값은 언제나 undefined

      ```javascript
      const numbers = [1,2,3];
      const powers = [];
      numbers.forEach( x => powers.push(x ** 2));

      const mult = numbers.map(x => x * 2);
      ```
  * map 반환값은 콜백함수의 반환 값들로 저장된 새로운 배열을 반환
* Array.from: 파라미터로 받은 유사 배열이나, iterable을 shallow coy를 통해 새로운 array를 만드는 method
*   2차원 배열

    ```javascript
    console.tables([
      ['고양이', 1],
      ['강아지', 2],
      ['송아지', 3],
    ]);
    // Array.from은 배열을 만들어 바로 element를 넣음
    Array.from(Array(row), () => new Array(col));
    Array.from({length: row}, () => new Array(col));
    / map은 fill을 실행시켜서 row 크기의 
    Array(row).fill(null).map(() => Array(col));
    ```

    * 추가, 삭제, 순회
*   slice(), splice(), split() 차이

    * slice(): 원본 배열 변경 X, 복사하고 새로운 배열로 리턴
    * splice(): 원본배열 변경 O, 잘려진 배열 반환, 원소 개수를 추가하거나 제거
    * split(): 문자열을 delimeter 기준으로 잘라서 배열로 전환

    ```javascript
    let arrayIntegers = [1, 2, 3, 4, 5];
    let arrayIntegers1 = arrayIntegers.slice(0, 2); // returns [1,2]
    let arrayIntegers2 = arrayIntegers.slice(2, 3); // returns [3]
    let arrayIntegers3 = arrayIntegers.slice(4); //returns [5]

    let arrayIntegersOriginal1 = [1, 2, 3, 4, 5];
    let arrayIntegersOriginal2 = [1, 2, 3, 4, 5];
    let arrayIntegersOriginal3 = [1, 2, 3, 4, 5];

    let arrayIntegers1 = arrayIntegersOriginal1.splice(0, 2); // returns [1, 2]; original array: [3, 4, 5]
    let arrayIntegers2 = arrayIntegersOriginal2.splice(3); // returns [4, 5]; original array: [1, 2, 3]
    let arrayIntegers3 = arrayIntegersOriginal3.splice(3, 1, "a", "b", "c"); //returns [4]; original array: [1, 2, 3, "a", "b", "c", 5]

     let testStr = "Hello, This is my world";
     testStr.split(" ");
     testStr.split("");
     let testStrFromArr = ["Hello", "test"].toString(); // "Hello,test"
    ```

#### String

```javascript
   let url = document.location.href;
   let qs = url.substring(url.indexOf('?') + 1).split('&');
   for(var i = 0, result = {}; i < qs.length; i++){
        qs[i] = qs[i].split('=');
        result[qs[i][0]] = decodeURIComponent(qs[i][1]);
    }
```

### Object, Array, Map, Set 차이

https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Keyed\_collections#maps

* 둘 다 iterable(순회 가능), key-value 쌍, key를 가지고 값을 조회, 삭제, 추가 Object
  * prototype을 가지고 있어서, 중복 key를 가질 수 있음 Map
  * Key를 가지고 정렬 가능
  * key를 사용해 잦은 추가, 삭제에 좋은 성능을 보임

### use strict

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Strict\_mode

* 암묵적으로 무시되던 에러를 표시
* 선언되지 않은 변수에 값 할당하면 referece error
* 매개변수 이름 중복하면 syntax error
* 일반 함수로 호출하면 this를 undefined로 저장
  * 왜 문제?
* new로 호출하여 생성자 함수로 사용한 경우는 해당 함수가 생성한 객체가 바인딩
* third party library는 strict mode 적용이 안 되어 있어서, 전체 파일에 적용하는건 권장하지 않음

### Hoisting

**변수 hoisting**

* var을 사용했을때 문제?
  * 값 오염
* var: 함수 scope, 재선언, 재할당 가능
* let: block scope, 재선언, 재할당 가능
* const: block scope, 선언과 초기화(값 할당)를 동시에 해야함. const a = 3; 안하면 syntax error
* var 변수, 함수 선언식은 scope 맨 윗줄로 올라가 선언되는 것처럼 동작, 런타임 이전에 JS 엔진에서 변수 선언과 (undefined로) 초기화를 동시에 실행

```javascript
console.log(a+b); // NaN
var a = 1;
var b = 2;

console.log(a+b); // ReferenceError
let a = 1;
let b = 2;

  function userDetails(username) {
    if (username) {
      console.log(salary); // undefined due to hoisting
      // Temporal dead zone (let 변수 초기화 이전에 접근하면 reference error 발생)
      console.log(age); // ReferenceError: Cannot access 'age' before initialization 
      let age = 30;
      var salary = 10000;
    }
    console.log(salary); //10000 (accessible to due function scope)
    console.log(age); //error: age is not defined(due to block scope)
  }
  userDetails("John");
```

* Temporal dead zone https://ui.toast.com/weekly-pick/ko\_20191014
  * let, const 경우, accessing a let or const variable before its declaration (within its scope) causes a Reference Error.

**함수 hoisting**

```
#### 선언(declaration), statement, 표현식(expression) 차이
```

* 함수 선언식은 함수 hoisting되지만, 표현식은 hoisting 안 된다.

#### 연산자

Incremet operator

```javascript
  let counter = 2;
  const preIncrement = ++counter;
  // counter = counter + 1;
  // preIncrement = counter;
  console.log(preIncrement); // 3

  counter = 2;
  const postIncrement = counter++;
  postIncrement = counter;
  counter = counter + 1;
  console.log(postIncrement); // 2
```

**Logical operators**

1. || (or 연산자) : 좌항이 true 여야 우항 연산 수행, 무거운 연산은 뒤로 보냄
2. && (and 연산자): 좌항이 false면 우항 연상 수행, 무거운 연산 뒤로
3. ! (not 연산자)

* https://developer.mozilla.org/ko/docs/Glossary/Falsy

1. Equality

* \== 연산자는 양쪽의 타입이 다른 경우, 타입을 강제로 형변환한뒤 비교한다

```javascript
console.log('5' == 5); // true
console.log(true == '1'); // true
console.log('5' === 5) // false
0 == false   // true
0 === false  // false
1 == "1"     // true
1 === "1"    // false
null == undefined // true
null === undefined // false
'0' == false // true
'0' === false // false
[]==[] or []===[] //false, refer different objects in memory
{}=={} or {}==={} //false, refer different objects in memory
```



### 고차함수(High-Order Function)

* 함수를 파라미터로 받거나, 함수를 반환하는 함수
* Array의 map, forEach, filter, reduce 모두 고차함수



### JS 엔진 (V8)이 개발되면서 JS 코드를 웹을 벗어나서도 사용 가능

https://d2.naver.com/helloworld/59361

* 웹워커: https://tech.kakao.com/2021/09/02/web-worker/
* JS코드를 머신코드로 컴파일(JIT;Just-In-Time) compiler
* Runtime(JS 동작되는 환경): web brower, Node.js
* 이벤트루프: JS 런타임 환경 기능, Call stack, callback Queue를 바라보고, call stack이 비어있을 경우 queue에서 콜백함수를 가져와 call stack에 넣어 JS engine이 실행할 수 있도록 기능

### JS 싱글 쓰레드와 이벤트 루프

https://poiemaweb.com/js-event#2-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84event-loop%EC%99%80-%EB%8F%99%EC%8B%9C%EC%84%B1concurrency

* 브라우저 안에는 JS engine(memory heap, call stack), WebAPIs(DOM, AJAX, Timeout..), callback queue, event loop가 있다.
* Memory Heap: 할당된 변수, 객체 들이 저장되는 영역
* Call Stack: 실행될 함수가 쌓이는 공간
* Web APIs: DOM Event, AJAX, Timer Event와 같은 사용자 인터렉션 또는 비동기 수행
* Task Queue: 비동기 함수들이 완료된 후 실행될 콜백들이 담긴 공간

### JS 메모리관리

https://developer.mozilla.org/ko/docs/Web/JavaScript/Memory\_Management

### 비동기 처리&#x20;

* 작업의 흐름이 멈추지 않고, 동시에 여러 가지 작업을 처리하거나, 기다리는 과정에서 다른 함수도 호출
  * WebAPI(서버쪽에 데이터를 요청하고 응답할때까지 대기하는 작업을 비동기적으로 처리)
  * 파일 읽기
  * 암호화/복호화
  * 스케쥴링(어떤 작업을 주기적으로 처리)

#### Callback

* a 함수 끝난 다음에 b 함수를 처리하고 싶을때는 콜백 함수(함수 타입의 값을 파라미터로 넘겨줘서, 파라미터로 받은 함수를 특정 작업이 끝나고 호출을 해주는 것을 의미)
* setTimeout 함수: 첫 번째 파라미터에 넣은 함수를 두 번째 파라미터에 넣은 시간(ms)이 흐른 뒤에 호출
* 우리가 정한 작업이 백그라운에서 수행되기 때문에 기존의 코드 흐름을 막지 않고 동시에 다른 작업들을 진행 할 수 있다.
* 비동기 작업이 많아질 경우, callback hell (코드의 깊이가 깊어지는 현상) 발생하여 가독성 나빠짐
* callback 중 발생한 에러 처리가 어려움

#### Promise (ES6)

https://poiemaweb.com/es6-promise

* 코드의 깊이가 깊어지는 현상 방지
* Promise 객체의 콜백함수로 resolve, reject 를 등록. resolve는 비동기 처리 로직이 완료될때 수행, reject는 실패될때 수행
* new Promise()로 프로미스를 생성하고 종료될 때까지 3가지 상태를 갖습니다.
  * Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
  * Fulfilled(이행) : 비동기 처리가 완료 resolve()되어 프로미스가 결과 값을 반환해준 상태. then()을 이용하여 결과 값을 받을 수 있음
  * Rejected(실패) : 비동기 처리가 실패 reject()하거나 오류가 발생한 상태. catch()를 이용하여 에러 객체를 받아서 핸들링
* 완료 여부와 관계 없이 finally() 호출
* then()으로 promise chaining(연결) 사용
  * then() 호출 결과는 Promise. 결과 값을 다음 비동기 작업에서 사용하는 경우, .then()을 붙여서 연달아 사용
* Promise.all
  * 병렬적으로 Promise
  * 순서를 보장하며, 하나라도 실패시 실패

#### async, await

https://joshua1988.github.io/web-development/javascript/js-async-await/

* 함수의 선언할때 async 라는 키워드를 붙입니다.
* 그러고 나서 함수의 내부 로직 중 HTTP 통신을 하는 비동기 처리 코드 앞에 await 키워드를 붙입니다. 여기서 주의하셔야 할 점은 비동기 처리 메서드가 꼭!! 프로미스 객체를 반환해야 await가 의도한 대로 동작합니다. (해당 프라미스가 끝날때까지 기다렸다가 다음 작업을 수행) 일반적으로 await의 대상이 되는 비동기 처리 코드는 Axios 등 프로미스를 반환하는 API 호출 함수입니다.
* async 함수 결과 값으로 Promise를 반환.
* 에러를 잡아낼때는 await 함수를 호출하는 부분에 try-catch 감싸서 사용
* Promise 보다 좋은 점

#### fetch vs. axios

### Generator

* 함수의 실행을 중간에 멈추고 재개할 수 있는 기능, 실행을 멈출 때 값을 전달할 수 있어서 반복문에서 하나씩 값을 꺼내서 사용할 수 있다
* function\*, yield 키워드를 사용해서 구현, next, return, throw 메소드를 갖고 있음

### debounce(), throttle()

https://webclub.tistory.com/607

* 둘 다 event 핸들러가 많이 발생하는 경우 제약을 걸어서 더 적게 이벤트를 발생하도록 제어
* debounce: 연달아 발생하는 이벤트를 그룹화해서 특정 시간이 지난 후 하나의 이벤트만 발생하도록 제어
  * 사용자가 브라우저 window 크기를 조정하는데, 멈출때까지 기다렸다가 resize event
  * input element에 사용자 input에 따라 검색어 추천하는 경우, 사용자가 키보드 입력을 멈출때까지, ajax(서버 요청) 이벤트를 발생하지 않는 경우
* throttle: 이벤트를 일정한 주기마나 발생시키도록 제어
  * 무한 스크롤 기능에서, footer에 도달하기 전에 contents를 가져와야하는데 debounce는 사용자가 스크롤을 멈출때 이벤트를 발생시키므로 throttle을 통해 사용자 위치가 얼마나 footer로 떨어져있는지 계산해서 contents를 가져오도록

### Event

https://ingg.dev/event-delegation/

* 버블링: 특정 element에 이벤트가 발생하면, root element까지 이벤트가 전달
* 캡쳐링: 특정 element에 이벤트가 발생하면, root element부터 이벤트가 전달.
* 이벤트 핸들러에 event.stopPropagation() 이벤트 수식어(preventDefault, stopPropagation) 추가: 이벤트 버블링, 캡쳐링 전파 모두 막음
  * event.target: 이벤트를 발생시킨 element (this = event.currentTarget) 현재 실행 중인 이벤트 핸들러가 할당된 element
  * 버블링: target element만 이벤트 발생
  * 캡쳐링: target element 기분으로 최상단 element만 이벤트 발생
* 위임(Delegation): 여러 하위 element에 이벤트 핸들러를 할당(addEventListener)하지 않고, 공통되는 부모 element에 이벤트를 할당하여 관리


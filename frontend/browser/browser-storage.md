---
description: Browser Storage 중 Cookie와 Web storage에 대한 설명입니다.
---

# Browser Storage

### Cookie, Web storage

#### **목적**

* HTTP는 "connectionless, stateless" 특성을 가지기 때문에 서버가 클라이언트가 누군지 매번 확인
  * 이 특성을 보완하기 위해 쿠키, Web storage, 세션 사용
  * 세션은 서버 자원을 사용하기 때문에 사용자가 많아지면 매우  부하를 일으
    * 이를 보완하기 위해 JWT(JSON Web Token) 사용

#### **Cookie, Web storage 공통점**

* 서버 DB를 사용하지 않고, 데이터를 클라이언트(브라우저)에서 임시 용도로 저장하기 위해서 사용
* 임시 용도의 데이터, cache, history 기능을 위해 사용
* 보안 문제가 될 만한 것들을 저장하지 않도록

#### **Cookie**

* 동작 방식
  * 클라이어트가 페이지 요청
  * 서버에서 쿠키를 생성하여, response header에 'Set-Cookie:key=value;' property 사용해서 쿠키를 포함시켜서 응답
  * 브라우저가 자동으로 Request Header에 쿠키를 넣어서 서버에 전송
    * 위에도 설명했지만 서버는 클라이언트에 대한 정보를 가지고 있지 않기 때문
  * 서버에서 쿠키를 읽어 이전 상태 정보를 변경할 필요가 있을때 쿠키를 업데이트하여 변경된 쿠키를 response header에 다시 설정
* 같은 도메인 상에서 쿠키 값은 공유
* 하나의 쿠키는 4KB까지 저장 가능
* key, value 값이 들어 있는 문자열 데이터 파일
* 이름, 값, 유효시간(쿠키 유지시간), 도메인(쿠키를 전송할 수 있는 도메인), Path(쿠키를 전송할 수 있는 경로) 필요
* 어떤 경우에 사용하나?
  * 필수적인 경우(이용자 동의 없이 저장됨): 페이지 탐색, 웹사이트 보안 영역 접속, 검색을 포함한 기본적인 기능 활성화
  * 기능: 접속자 지역(timezone), 언어 등 UI에 영향을 줄 수 있는 설정을 저장, 사용자 설정에 따라 UI 변경
  * 성능: 정보의 익명 수집 및 보고를 통해 웹사이트 최적화
    * Walkme 스크립트를 실행하기 위해서 성능 쿠키 수집 팝업 구현
  * 마케팅: 사용자의 방문 내역 추적하여 사용자 경향, 웹사이트 이용 패턴을 파악하여 관련있는 정보, 광고 제공

#### **Web storage**

* HTML5에서 제공, 쿠키는 문자열이지만 web storage는 객체 정보 저장이 가능해서 개발 편의성 향상
* 쿠키와 달리 서버에 자동 전송되지 않음
* 브라우저 마다 차이가 있으나 모바일 2.5MB, 데스크탑 5\~10MB 저장 가능
* 만료 기한에 따라 Local storage, Session Storage 분류
* Origin에 묶여있기 때문에, 다른 protocol, domain에 접근 불가능

**Local storage**

* 데이터 만료 기한이 없어, 브라우저에 영구적으로 보전
  * 사이트 재 방문시, 브라우저를 닫고 열어도 저장되었던 데이터 이용 가능
* 다른 브라우저, 다른 탭 접근 가능
* 사용 예
  * 사용자 언어 설정
  * 최근 검색 결과
  * 자동 로그인
  * '오늘 팝업을 열지 않음'
  * 사이트 언어 설정

**Session storage**

* 브라우저, 탭 종료하면 데이터 삭제
* 해당 브라우저, 해당 탭 접근 가능
* 사용 예
  * 공지사항 알림 확인 여
  * 탭 이동시 사용자의 이동경로
  * 입력 form 작성 중간에 임시 저장된 데이터
  * 쇼핑몰의 장바구니

**Ref.**

* https://ko.javascript.info/localstorage
* https://ykss.netlify.app/web/storage\_session\_cookie/
* https://l4279625.tistory.com/entry/Cookie%EC%99%80-Web-Storage

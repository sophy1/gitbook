# Style

Basic

* display
  * inline-block, inline, block, flex none..
  * visibility 차이
* 위치
  * top, left, bottom, right
  * flex-direction
  * z-index
  * transform, opacity
* box
  * padding
  * margin
  * border
  * width
  * height
  * background-color
  * align-items
* span
  * font-size
  * font-weight
  * color
  * text-overflow
  * white-space
  * word-break: keep-all;
  * text-align
* @font-face
  * font-family: 'NotoSansKR'
  * src: url('../fonts/NotoSansKR/notokr-regular.woff2') format('woff2')
  * font-weight
  * font-style
* icon
  * background-image
  * background-size
  * border-radius
* scroll
  * overflow-x: auto;
  * scrollbar-width
* category
  * display:flex;
  * frex-wrap: wrap;
  * justify-content: space-between;
  * align-items: center;
  *



반응형(responsive)

```css
@import './responsive/pc.css';
@import './responsive/tablet.css' screen (max-width: 1200px);
@import './responsive/mobile.css' screen (max-width: 768px);

/* 변수 */
:root {
  --app-height: 100%;
  --header-height-home: 52px;
  
  /* 공통 color */
  --primary-color: #364fc7;
  
  --status-update: #f7a443;
  --status-new: #fb4d58
} 

/* 변수 사용법 */
.contact a:hover { color: var(--primary-color); }
.code-wrap { border: 1px solid; }

/* 재정의 */
@media (max-width: 768px) {
  .container {
    width: 100%;
  }
}
```

#### Rendering engine (브라우저 호환성, 크로스브라우징)

```css
/* CSS prefix (-moz, -webkit, -ms)*/

header .menu-inner ul {
  display: flex;
  align-items: center;
  width: fit-content;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-drag: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}

/*
  https://developer.mozilla.org/en-US/docs/Web/CSS/::-webkit-scrollbar
 */
.tab nav::-webkit-scrollbar {
  display: none;
}
.card-list {
  -ms-overflow-style: none; /* Hide scrollbar for IE and Edge */
  scrollbar-width: none; /* Hide scrollbar for Firefox */
}
```

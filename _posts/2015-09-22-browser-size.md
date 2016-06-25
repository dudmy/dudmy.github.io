---
layout: post
title: "브라우저의 창 크기 구하기"
categories:
  - JavaScript
comments: true
---

현재 브라우저의 창 크기를 JavaScript의 Browser 객체를 이용해 구할 수 있다.

### XHTML 버전
표준이 없어 환경에 따라 다른 결과가 나올 수 있다.

```html
/* 창의 너비와 높이 */
document.body.clientWidth
document.body.clientHeight

/* 문서 전체의 너비와 높이 */
document.body.scrollWidth
documnet.body.scrollHeight 
```

### HTML5 버전
표준은 있지만 IE 구 버전에서 안될 수 있다.

```html
/* 브라우저 UI(윈도우 두께)를 제외한 실질적 너비와 높이 */
window.innerWidth
window.innerHeight

/* 브라우저 UI(윈도우 두께)를 포험한 전체 너비와 높이 */
window.outerWidth
window.outerHeight
```

### 자바스크립트에서 사용 예

```javascript
var size = {
    width : window.innerWidth || document.body.clientWidth,
    height : window.innerHeight || document.body.clientHeight 
}
```


---
layout: post
title: "자바스크립트의 함수 (function)"
categories:
  - JavaScript
tags:
  - javascript
  - jquery
  - study
---

바스크립트에서 함수를 설명할 때 first-class object(또는 citizen, value)라고 한다. 함수는 객체를 의미하고 변수, 배열, 객체에 저장될 수 있다는 뜻이다.

```javascript
// 함수 선언식(function declaration)
function test() {

}
```

함수 선언식은 스크립트가 로딩되는 시점에서 초기화를 하고 이를 VO(variable object)에 저장한다. 따라서 함수 선언 위치와 상관없이 소스 내 어느 곳에서든 호출이 가능하다.

```javascript
// 익명 함수 표현식(anonymous function expression)
var test = function() {

}

// 함수 호출
test();
```

익명 함수 표현식은 함수를 정의하고, 변수에 함수를 저장하고 실행하는 과정을 거친다. 따라서 "함수는 객체이다." 라는 정의가 가능하다. 함수 선언식과는 다르게 Runtime 시에 함수가 해석되고 실행된다.

```javascript
// 익명 즉시 실행 함수(immediately-invoked function expression)
(function() {

}());
```

즉시 실행 함수는 함수 표현식과는 다르게 일련의 과정을 거치지 않고 즉시 실행된다. 또한 Global Namespace에 변수를 추가하지 않아도 되기 때문에 코드 충돌 없이 구현할 수 있어 플러그인이나 라이브러리 등을 만들 때 많이 사용된다.

```javascript
(function(W, D) {

}(window, document));
```

위 코드는 jQuery plugin 작성 시에 적용하는 기본적인 초기 구조이다. 함수에 window, document와 같은 전역변수를 굳이 넘기는 이유는?

1. Local 변수에 캐싱하여 효과를 높일 수 있다.
2. 코드 압축 수행시 효과적으로 압축이 가능하다.

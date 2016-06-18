---
layout: post
title: "자바스크립트 정규식을 사용한 문자 치환"
categories:
  - JavaScript
tags:
  - javascript
  - study
  - programming
  - regular expression
---

문자열로 되어있는 시간의 차이를 구해야 하는 일이 있었다. 나는 replace 함수를 이용해 문자를 치환하여 값을 비교하는 방법으로 진행했다.

```javascript
var time = 06:25:11
```

### .replace()

```javascript
time.replace(':', ''); // 0625:11
```

자바스크립트에서 replace 함수는 맨 처음 문자 하나만 치환한다. 모든 문자를 치환하는 자바의 replaceAll 같은 함수가 없다. 대신 정규식 표현을 이용하면 같은 효과를 얻을 수 있다.  
　

### 정규식 사용

```javascript
time.replace(/:/gi, ''); // 062511
```

* g: 발생할 모든 패턴에 대한 전역 검색 (Global search)
* i: 대/소문자 구분을 무시 (Case-insensitive search)

정규식(Regular Expression)은 문자열에서 문자 조합에 일치 시키기 위하여 사용되는 형식 언어이다. 주로 검색, 치환, 추출을 위한 패턴을 표현하는 데 사용된다. 자세한 설명과 특수문자는 [MDN - JavaScript 정규식](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/%EC%A0%95%EA%B7%9C%EC%8B%9D) 에서 확인할 수 있다.  
　

### 마스킹 처리

이번에는 정규식을 활용하여 문자열 마스킹 처리를 해본다. 예를 들면, 정보 보호를 위하여 주민등록번호를 123456 - 7**** 처럼 표현하는 것?

```javascript
/*
 * 마지막 글자를 * 처리한다. (ex. 홍길동 → 홍길*)
 */ 
function maskingName(strName) {
    if(strName === undefined || strName === '') {
        return '';
    }
    var pattern = /.$/; // 정규식
    return strName.replace(pattern, "*"); 
}

/*
 * 뒤에서 부터 3글자를 * 처리한다. (ex. 12가3456 → 12가3***)
 */
function maskingCar(strCar) {
    if (strCar == undefined || strCar === '') {
        return '';
    }
    var pattern = /.{3}$/; // 정규식
    return strCar.replace(pattern, "***");
}
```

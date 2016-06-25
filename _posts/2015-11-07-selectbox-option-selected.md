---
layout: post
title: "select box의 option 찾기"
categories:
  - JavaScript
comments: true
---

selected 된 option을 찾는 것이 아니라, 특정한 값을 이용해 해당 option을 찾아서 선택한다. 사용할 때마다 잊어먹어서 정리해 놓는다.

```html
<select id='testSelect'>
    <option value='1' testDay='20151107-1'>일</option>
    <option value='2' testDay='20151107-2'>이</option>
    <option value='3' testDay='20151107-3'>삼</option>
</select>
```

### value 값으로 찾아서 선택하기

```javascript
var test = "2";
$('#testSelect option[value='+ test +']').attr('selected', true);
```

### text 값으로 찾아서 선택하기

```javascript
var test = "이";
$('#testSelect option:contains('+ test +')').attr('selected', true);
```

### 임의의 값으로 찾아서 선택하기

```javascript
var test = "20151107-2";
$('#testSelect option[testDay="'+ test +'"]').attr('selected', true);
```

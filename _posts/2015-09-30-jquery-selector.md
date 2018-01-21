---
layout: post
title: "제이쿼리 선택자 정리"
categories: javaScript
comments: true
---

jQuery에서 태그와 같은 특정 객체를 선택하기 위해서 Selector(선택자)를 이용한다. 선택자의 종류가 다양하지만 경험이 적어 우선 사용해 본 선택자에 대해서만 정리해본다. 추후 하나씩 공부하면서 추가해나간다.

```html
<div>
    <ul>
        <li> child1-1 </li>
        <li> child1-2 </li>
        <ul>
            <li> child2-1 </li>
            <li> child2-2 </li> 
        </ul>
    </ul>
</div>
```

### .find()
특정 노드의 하위 노드에서 인자의 요소를 찾는다.

```javascript
$('div').find('li'); // 모든 하위 요소인 child1과 child2 모두 검색된다.
```

### .children()
특정 노드의 직계 하위 노드에서 인자의 요소를 찾는다.

```javascript
$('div').children('li'); // 바로 아래의 하위 요소인 child1만 검색된다.
```

### .eq()
특정 노드의 n번째에 위치하는 인자의 요소를 찾는다.

```javascript
$('li').eq(0); // 모든 li 중에서 첫 번째 요소인 child1-1만 검색된다.
$('li:eq(2)'); // 모든 li 중에서 세 번째 요소인 child2-1만 검색된다.
```


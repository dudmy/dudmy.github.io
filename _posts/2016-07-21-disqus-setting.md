---
layout: post
title: "GitHub Pages에 comment 기능 추가하기"
categories:
  - Etc
comments: true
---

GitHub Pages는 Jekyll에 의해 구동되는 정적 웹 페이지(Static Web Page)이다. 그래서 일반 블로그와 같은 동적 웹 페이지(Dynamic Web Page)의 서비스들이 없다. 하지만 흔히 소셜 댓글 서비스라고 불리는 Disqus를 이용하면 정적 웹 페이지에서도 comment를 달 수 있다.  

　  
## Disqus 설정하기

1.　[Disqus](https://disqus.com/) 가입하기 & Verify email 인증하기

![gps-4]({{ site.url }}/public/post/20160702/gps-4.png)

2.　Settings > Edit Profile > 기본 정보들 입력하기 > Save

**Website**: 이곳에 자신의 GitHub Pages 주소를 적는다. (중요)

![gps-5]({{ site.url }}/public/post/20160702/gps-5.png)

3.　Settings > Add Disqus To Site > 맨 아래에 GET STARTED

Site name & Disqus URL & Category 를 설정하고 Next 버튼 클릭한다.
설정 과정에서 중요한 부분은 아니므로 큰 고민을 할 필요는 없다. (Disqus 내에서 보여지는 정보들)

4.　Disqus에 대한 설명하는 페이지가 나온다 > Yes, I understand ... 클릭 > 플랫폼 선택 페이지에서 GitHub가 없으므로, Universal Code를 클릭한다.

![gps-6]({{ site.url }}/public/post/20160702/gps-6.png)

6.　첫 번째 코드가 Disqus를 페이지에 load하는 작업이다. 해당 code를 복사하여 자신의 GitHub Pages > _layouts > post.thml 에 붙여넣는다. 그러면 작업 완료!

```html
<div class="post">
  ... 생략 ...
</div>

<!-- disqus start -->
<div id="disqus_thread"></div>
<script>
    ... 생략 ...
</script>
<!-- disqus end -->
```

두 번째 코드는 comment 개수를 카운트하는 작업이다. 딱히 이 기능은 필요가 없어서 추가하지 않았다. 사용 방법은 해당 페이지에 나와있으니 그대로 하면 잘 동작할거라 생각된다.

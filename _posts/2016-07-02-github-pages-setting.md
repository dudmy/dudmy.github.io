---
layout: post
title: "GitHub Pages 사용을 위한 세팅하기"
categories: etc
comments: true
---

내용이 계속 추가되면 포스팅이 너무 길어질거 같아서... 추가되는 내용들은 링크로 대체한다.

## Sitemap 제출하기

![gps-1]({{ site.url }}/assets/post/20160702/gps-1.png)

위 포스트의 주소는 [http://dudmy.net/android/2016/02/14/list-item-ripple-effect/](http://dudmy.net/android/2016/02/14/list-item-ripple-effect/) 인데, 보는 것과 같이 사이트가 제대로 크롤링 되지 않는 문제가 있다. 정확한 이유인지는 모르겠지만 Sitemap을 제출하지 않는 것으로 짐작된다. [Sitemaps for GitHub Pages](https://help.github.com/articles/sitemaps-for-github-pages/) 등을 참고하여 진행해보았다.

1.　_config.yml 파일에 Automatic sitemap generation을 위해 추가한다.

```yml
gems:
    - jekyll-sitemap
```

2.　local에서 test를 해보면, 아래와 같이 _site 폴더에 sitemap.xml 파일이 생성된걸 확인할 수 있다.

![gps-2]({{ site.url }}/assets/post/20160702/gps-2.png)

3.　push 한 다음날 확인해보니 정상적으로 크롤링 되는 것을 확인.

![gps-3]({{ site.url }}/assets/post/20160702/gps-3.png)


## Disqus 설정하기 (comment 기능 추가)

[포스트로 이동하기 >]({{ site.url }}/etc/2016/07/21/disqus-setting/)
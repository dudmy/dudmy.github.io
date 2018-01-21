---
layout: post
title: "워드프레스 검색엔진 최적화 설정하기"
categories: etc
comments: true
---

> 검색엔진 최적화란?
> 
> 검색엔진 최적화 (Search Engine Optimization, SEO)는 웹 페이지 검색엔진이 자료를 수집하고 순위를 매기는 방식에 맞게 웹 페이지를 구성해서 검색 결과의 상위에 나올 수 있도록 하는 작업을 말한다. 웹 페이지와 관련된 검색어로 검색한 검색 결과 상위에 나오게 된다면 방문 트래픽이 늘어나기 때문에 효과적인 인터넷 마케팅 방법 중의 하나라고 할 수 있다. 기본적인 작업 방식은 특정한 검색어를 웹 페이지에 적절하게 배치하고 다른 웹 페이지에서 링크가 많이 연결되도록 하는 것이다. - 위키백과 -


### 구글 웹마스터 도구에 사이트 등록

Google Search Console(구글 웹마스터 도구)은 사용자가 사이트의 Google 검색결과 순위를 모니터링하고 관리할 수 있도록 돕기 위해 Google에서 무료로 제공하는 서비스이다. Search Console에 가입하지 않아도 Google 검색결과에 사이트가 표시되긴 하지만, 가입하면 Google에서 사이트가 어떻게 검색되는지를 알 수 있어 검색결과 순위를 극대화할 수 있다.

1.　[구글 웹마스터](https://www.google.com/webmasters/#?modal_active=none)에서 Search Console에 로그인

![wss-1]({{ site.url }}/assets/post/20150716/wss-1.png)

2.　관리하고자 하는 사이트 주소 입력 → 속성 추가 클릭

![wss-2]({{ site.url }}/assets/post/20150716/wss-2.png)

3.　권장 방법인 HTML 태그를 사이트 홈페이지에 추가 → 확인 클릭

워드프레스에서 외모 - 테마편집기 - header.php 템플릿을 선택한다. 그리고 <head></head> 태그 사이에 복사한 메타태그를 붙여 넣는다. 파일 업데이트를 한 이후에 Search Console에서 확인 버튼을 누른다.

![wss-3]({{ site.url }}/assets/post/20150716/wss-3.png)

4.　사이트 소유자 확인 성공

![wss-4]({{ site.url }}/assets/post/20150716/wss-4.png)

5.　워드프레스에서 Google XML Sitemaps 플러그인 설치

사이트맵을 Search Console에 제출해야 한다. 사이트맵은 사이트의 웹 페이지를 나열하는 파일로 사이트 콘텐츠의 구성을 Google 및 다른 검색 엔진에 알리는 데 사용된다. Googlebot과 같은 검색 엔진 웹 크롤러는 이 파일을 읽고 사이트를 더 지능적으로 크롤링한다.

![wss-5]({{ site.url }}/assets/post/20150716/wss-5.png)

6.　설정에서 XML-Sitemap 클릭→ 사이트맵 index file의 URL 복사

![wss-6]({{ site.url }}/assets/post/20150716/wss-6.png)

7.　Search Console - 대시보드에서 Sitemaps >> 클릭

![wss-7]({{ site.url }}/assets/post/20150716/wss-7.png)

8.　우측 상단의 SITEMAP 추가/테스트 클릭 → 복사한 URL 붙여넣기 → Sitemap 제출 클릭

![wss-8]({{ site.url }}/assets/post/20150716/wss-8.png)

9.　사이트맵 제출 완료

보통 구글에서 색인(index)이 생성되는데 24시간 정도 걸린다고 한다.

![wss-9]({{ site.url }}/assets/post/20150716/wss-9.png)

추가) Google 검색의 원리는 크롤링과 색인 생성을 바탕으로 웹에서 정보를 수집하고 조직하여 가장 유용한 결과를 사용자에게 표시한다. 크롤링으로 정보 검색을 하고 색인 생성을 통해 정보 체계화를 한다. 자세한 내용은 [Google 검색 속으로](http://www.google.co.kr/intl/ko/insidesearch/) 참고.


### 워드프레스 검색엔진 최적화 플러그인

워드프레스 사용자들은 복잡한 코딩 없이 플러그인을 이용해 원하는 설정을 간단하게 할 수 있다. 이번에는 필수 플러그인 중 하나이며 검색엔진 최적화 플러그인 중 가장 인기있는 WordPress SEO by Yoast 를 이용한다.

1.　WordPress SEO by Yoast 플러그인(현재,  Yoast SEO) 설치 & 활성화

![wss-10]({{ site.url }}/assets/post/20150716/wss-10.png)

2.　글 편집 페이지 하단에 Yoast SEO 항목을 설정

각 항목에 알맞은 정보를 입력하여 검색엔진에 잘 노출될 수 있도록 한다.

* Snippet Preview: 미리보기로, 해당 글의 정보가 검색엔진에서 보이는 형태이다. 다른 항목들을 수정하면 미리보기도 이에 맞추어 바뀐다.
* Focus Keyword: 글의 주제 또는 핵심 키워드를 입력하는 부분이다. 예를 들어 본 글의 키워드는 '검색엔진 최적화' 이다. 해당 키워드를 입력하면 아래에 Focus Keyword usage(포커스 키워드 사용량)이 나타난다.
* SEO Title: 글의 제목이 검색엔진에서 보이는 형태이다. 따로 설정하지 않으면 기본 워드프레스 설정으로 되어있다. 제목이 길어 일부분이 짤릴 경우 수정하곤 한다.
* Meta description: 글의 간략한 설명이 검색엔진에서 보이는 형태이다. 따로 설정하지 않으면 자동으로 글의 일부분이 가져와진다.

![wss-11]({{ site.url }}/assets/post/20150716/wss-11.png)

3.　글을 업데이트 하면 녹색 불과 함께 SEO 상태가 나타난다. 불의 색깔은 설정 하지 않은 상태인 회색(N/A)부터 녹색(Good), 주황색(Poor), 빨강색(Bad) 등이 있다.

![wss-12]({{ site.url }}/assets/post/20150716/wss-12.png)

4.　위의 화면에서 Check를 클릭하면 아래의 화면으로 이동한다. 해당 글의 SEO 상태를 분석하여 알려준다. 이를 통해 부족한 부분을 추가하고 수정할 수 있다.

![wss-13]({{ site.url }}/assets/post/20150716/wss-13.png)

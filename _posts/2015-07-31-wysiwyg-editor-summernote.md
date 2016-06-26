---
layout: post
title: "오픈소스 위지윅 에디터, Summernote"
categories:
  - Etc
comments: true
---

학교에서 웹 개발 관련 프로젝트에 참여하고 있다. 게시판을 제작하던 중에 글 작성에 사용할 위지윅 에디터(WYSIWYG Editor)가 필요했다. 웹 개발 경험이 적어 단어 조차 처음 들어보았지만... 이번 기회에 에디터를 사용해본다.  
　

> 위지윅 에디터란?  
> WYSIWYG(What You See Is What You Get) Editor란 코드를 작성하는 대신 간편하게 실제 페이지 레이아웃과 유사한 형식으로 웹페이지를 작성할 수 있는 HTML 편집기의 한 종류를 말한다. - Google AdSense

사용할 위지윅 에디터는 국내에서 시작된 오픈소스 프로젝트로 한국 개발자들이 만든 에디터다. Super simple WYSIWYG Editor using Bootstrap 라는 설명만큼 간단한 설정만으로 사용할 수 있다.

![wes-1]({{ site.url }}/public/post/20150731/wes-1.png)  
　 

### WYSIWYG Editor 적용하기

1.　[summernote.org](http://summernote.org/#/) 에서 Download source code 클릭하여 다운

2.　자신의 프로젝트에 다운받은 CSS 파일과 JS 파일 넣기

```
CSS 파일: dist > summernote.css
JavaScript 파일: dist > summernote.min.js
메뉴를 한국어로 변환: lang > summernote-ko-KR.js
```

3.　HTML 파일에 필요한 라이브러리와 다운받은 파일들 추가

내 경우 CSS 파일은 <head> 태그 안에 추가하고, JS 파일은 웹 페이지의 빠른 로딩을 위해 <body> 태그 끝에 추가하였다.

```html
<!-- include libries(jQuery, bootstrap, fontawesome) -->
<script src="//code.jquery.com/jquery-1.9.1.min.js"></script> 
<link href="//netdna.bootstrapcdn.com/bootstrap/3.0.1/css/bootstrap.min.css" rel="stylesheet">
<script src="//netdna.bootstrapcdn.com/bootstrap/3.0.1/js/bootstrap.min.js"></script> 
<link href="//netdna.bootstrapcdn.com/font-awesome/4.0.3/css/font-awesome.min.css" rel="stylesheet">
```

자신의 프로젝트에서 css/js 파일이 위치한 경로(file-path)를 올바르게 입력해야 한다. 경로가 틀릴 경우 파일을 인식하지 못한다.

```html
<!-- include summernote css/js-->
<link href="file-path/summernote.css" rel="stylesheet">
<script src="file-path/summernote.min.js"></script>
```

4.　게시판의 글 작성에서 내용 부분의 textarea 변경

```html
<textarea class="summernote" name="contents"></textarea>
```

5.　마지막으로 높이와 언어 등의 옵션을 설정한 Initialize 코드 추가

```html
<script>
    $(document).ready(function() {
        $('.summernote').summernote({
            height: 300,
            lang: 'ko-KR'
        });
    });
</script>
```

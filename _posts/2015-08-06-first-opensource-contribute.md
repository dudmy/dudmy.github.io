---
layout: post
title: "처음으로 오픈소스에 기여하다"
categories:
  - Etc
tags:
  - opensource
---

[초보 개발자가 오픈소스에 기여하는 5단계](http://www.bloter.net/archives/197960) 에서는 오픈소스 SW를 개발하는 개발자에게 도움을 주는 모든 행위가 오픈소스에 기여하는 일이라고 한다. 오류 제보부터 문서화 작업에 참여하는 일까지 포함된다고 한다.

며칠 전 참여하고 있는 프로젝트에서 사용할 WYSIWYG Editor의 홈페이지를 둘러보고 있었다. 사용법에 대해 천천히 읽고 있었는데 한 글자가 눈에 들어왔다.

```html
<!-- include libries(jQuery, bootstrap, fontawesome) -->
<script src="//code.jquery.com/jquery-1.9.1.min.js"></script>
...
```

**"libries"...**

처음에는 이게 올바른 단어인가 고민했다. 평소에는 잘 보이지도 않던 오자가 갑자기 눈에 띄니 제가 잘 못 알고있나 했다. 사전 및 다른 문서들을 찾아본 결과 오자라고 확신한 후, 아주 작은 일이지만 알리기로 마음 먹었다.

우선 해당 GitHub Pages에서 'libries' 라고 표기된 오자를 'libraries'라고 수정하였다. commit 메시지는 무엇으로 할까 고민하다가 수정 내용에 대해서만 간략하게 적었다.

```
fix typo 'libries' to 'libraries'
```

GitHub 사용이 능숙한 편이 아니라 과정에서 미숙한 점이 많았다. 예를들면 branch 설정을 어떻게 해야할지 몰라서 master branch로 냅두고... 어쨋든 우여곡절 끝에 오픈소스에 처음으로 pull request를 보냈다. 혹여 merge 되지 않더라도 시도한 것에 의의를 두자며 마음을 비우고 있었다.

그런데 PR을 보낸지 얼마 지나지 않아서 메일이 왔다.

```
Merged #19.
_
Reply to this email directly or view it on GitHub.
```

노트북을 키고 GitHub에 들어가보니... 아까 보낸 PR이 Merged 됐다! 이렇게 컨트리뷰트가 받아들여지면서 처음으로 오픈소스 프로젝트에 작게나마 기여했다. :D

오자 하나 수정하는 작은 일로 오픈소스 프로젝트에 기여하는 신기한 경험이었고, 앞으로 다양한 오픈소스 프로젝트에 참여하도록 노력해야겠다.


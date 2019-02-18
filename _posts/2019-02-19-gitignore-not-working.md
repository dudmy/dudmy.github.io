---
layout: post
title: ".gitignore 동작하지 않을 때"
categories: git
comments: true
---

간혹 .gitignore 파일에 작성한 untracked 파일이 무시되지 않고 changes에 포함되는 경우가 있다.  
이를 해결하기 위해서 하드디스크에 있는 파일은 그대로 두고 Git만 추적하지 않도록 해야 한다.

즉 `git rm`으로 tracked 상태의 파일을 삭제한 후에 `git add`으로 다시 파일을 추가한다.  
이때 .gitignore 파일에 포함된 untracked 파일은 추가되지 않는다.

```
git rm -r --cached .
git add .
git commit -m "Fixed gitignore issue"
```

참고: [Git - git-rm Documentation](https://git-scm.com/docs/git-rm)
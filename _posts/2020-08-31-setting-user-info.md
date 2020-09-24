---
layout: post
title: "Git 저장소마다 다른 사용자 정보 설정하기"
categories: git
comments: true
---

Git을 설치하고 나서 사용자 정보(이름과 이메일 주소)를 설정할 수 있다.  
커밋 할 때마다 이 정보를 사용하며 모든 저장소에 적용되어 우리는 딱 한 번만 설정해 주면 된다.

```
$ git config --global user.name "dudmy"
$ git config --global user.email dudmy@example.com
```

만약 저장소마다 다른 사용자 정보를 사용하고 싶다면, 해당 저장소에서 --global 옵션을 빼고 설정해 주면 된다.

```
$ git config user.name "yujin"
$ git config user.email yujin@example.com
```

아래와 같이 사용자 정보가 정상적으로 설정된 것을 확인할 수 있다.

```
$ git config user.name
> yujin

$ git config user.email
> yujin@example.com
```

참고: [Setting your username in Git - GitHub Docs](https://docs.github.com/en/github/using-git/setting-your-username-in-git)

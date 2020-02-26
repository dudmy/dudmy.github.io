---
layout: post
title: "마지막 Commit Message 수정하기"
categories: git
comments: true
---

가장 최근의 커밋 메시지는 아래의 명령을 사용하여 변경할 수 있다.

```
git commit --amend
```

위 명령어에 -m을 추가하면 텍스트 편집기로 넘어가지 않고 메시지를 바로 변경할 수도 있다.

```
git commit --amend -m "new commit message"
```

GitHub에 이미 푸시 한 경우에는 수정된 메시지와 함께 커밋을 강제로 푸시해야 한다.
다만 강제로 푸시하면 repository history가 변경되어, 이미 clone 한 사람들에게 문제가 발생할 수 있는 점을 유의해야 한다.

```
git push --force
```

참고: [Changing a commit message - GitHub Help](https://help.github.com/en/github/committing-changes-to-your-project/changing-a-commit-message)
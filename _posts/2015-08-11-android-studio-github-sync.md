---
layout: post
title: "Android Studio와 GitHub 연동"
categories:
  - Android
comments: true
---

그동안의 프로젝트에서는 소스 코드를 GitHub에 수동으로 관리하였다. 예를 들면 어느 정도 진행한 후, 프로젝트 폴더를 복사하고 로컬 저장소에 있는 기존 폴더를 지우고 붙여넣는다. 마지막으로 add, commit, push 작업을 진행해 원격 저장소에 보낸다. 매우 귀찮고 비효율적인 방법이다...

그래서 이번에 개인 프로젝트를 하면서 Android Studio와 GitHub를 연동하여 효율적으로 관리하려고 한다. 아래의 과정을 진행하기 전에 우선 컴퓨터에 git이 설치되어 있어야 한다. [Git - Downloads](https://git-scm.com/downloads) 에서 OS에 맞는 git을 받을 수 있다.  
　

1.　GitHub에서 새로운 Repository 생성

![asgs-1]({{ site.url }}/assets/images/post/20150811/asgs-1.png)

2.　Android Studio에서 새로운 Project 생성

기존에 생성된 프로젝트로 진행해도 무관하다. 뒤의 과정은 생략...

![asgs-2]({{ site.url }}/assets/images/post/20150811/asgs-2.png)

3.　프로젝트 생성 또는 연 후, 상단 메뉴에서 VCS(Version Control System) → Import into Version Control → Create Git Repository... 클릭

![asgs-3]({{ site.url }}/assets/images/post/20150811/asgs-3.png)

4.　Git으로 관리할 프로젝트를 선택한 후 OK 클릭

'git init'은 기존 프로젝트를 Git으로 관리하고 싶을 때 실행하는 명령어이다.

![asgs-4]({{ site.url }}/assets/images/post/20150811/asgs-4.png)

5.　해당 프로젝트의 최상위 폴더에서 오른쪽 마우스 버튼 클릭 → Git Bash 클릭 → command 창에 명령어 입력

```
git remote add origin https://github.com/[username]/[repository_name].git
```

이는 원격 서버의 주소를 git에게 알려주기 위한 방법이다.  

username은 GitHub의 Username이고, repository_name은 1번에서 생성한 Repository name 이다. 예를 들면 나같은 경우 CheckitLife 프로젝트 폴더에서 Git Bash를 열고 아래의 명령어를 입력한다.

```
git remote add origin https://github.com/dudmy/CheckitLife.git
```

6.　프로젝트 오른쪽 버튼 클릭 → Git → Add 클릭 &  Git → Commit Directory... 클릭

소스 코드에 변동사항이 있을 경우, 해당 부분의 파일 색깔이 다르게 표시된다.

![asgs-5]({{ site.url }}/assets/images/post/20150811/asgs-5.png)

7.　Commit Message에 해당 커밋에 대한 설명(내용)을 입력한 후 Commit 클릭

![asgs-6]({{ site.url }}/assets/images/post/20150811/asgs-6.png)

8.　프로젝트 오른쪽 버튼 클릭 → Git → Repository → Push 클릭

![asgs-7]({{ site.url }}/assets/images/post/20150811/asgs-7.png)

9.　입력한 Commit Message 확인 후 Push current branch... 체크 → Push 클릭

여기서는 기본 설정인 master branch를 이용한다.

![asgs-8]({{ site.url }}/assets/images/post/20150811/asgs-8.png)

10.　GitHub 로그인을 위한 정보 입력 후 OK 클릭

![asgs-9]({{ site.url }}/assets/images/post/20150811/asgs-9.png)

11.　Push 성공 및 Android Studio와 GitHub 연동 완료

해당 프로젝트의 Repository에 가보면 소스 코드가 원격 저장소에 Push 된 것을 확인할 수 있다.

![asgs-10]({{ site.url }}/assets/images/post/20150811/asgs-10.png)


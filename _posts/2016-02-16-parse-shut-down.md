---
layout: post
title: "Parse.com 서비스 종료"
categories:
  - Chatter
comments: true
---

1월 29일 한 통의 메일이 날라왔다. Facebook이 인수한 클라우드 기반의 백엔드 인프라를 제공하는 서비스인 Parse에서 온 메일이었다. 새로운 서비스 소개나 광고인줄 알고 넘겼는데, 나중에 읽어보니 생각지도 못한 내용이었다...

> We have a difficult announcement to make. Beginning today we're winding down the Parse service, and Parse will be fully retired after a year-long period ending on January 28, 2017. We're proud that we've been able to help so many of you build great mobile apps, but we need to focus our resources elsewhere.
> 
> We understand that this won't be an easy transition, and we're working hard to make this process as easy as possible. We are committed to maintaining the backend service during the sunset period, and are providing several tools to help migrate applications to other services.
> 
> First, we're releasing a database migration tool that lets you migrate data from your Parse app to any MongoDB database. During this migration, the Parse API will continue to operate as usual based on your new database, so this can happen without downtime. Second, we're releasing the open source Parse Server, which lets you run most of the Parse API from your own Node.js server. Once you have your data in your own database, Parse Server lets you keep your application running without major changes in the client-side code. For more details, check out our migration guide here.
> 
> We know that many of you have come to rely on Parse, and we are striving to make this transition as straightforward as possible. We enjoyed working with each of you, and we have deep admiration for the things you've built. Thank you for using Parse.

...

한마디로 요약하면 하면 Parse 서비스 종료 :(

과거에 팀 프로젝트를 진행하면서 본 서비스를 처음 알게 되었고 유용하게 사용했는데 종료라니... 지난 프로젝트는 더 이상 쓰지 않기 때문에 문제 되는 부분이 없었지만, 진행하고 있던 본 CheckitLife 개인 프로젝트가 생각났다.

App 개발에 집중하기 위해 서버와 데이터베이스를 BaaS로 구축했는데. 음... 물론 현재는 진행 보류인 상태이지만, 나중에 다시 착수하게 된다면 새로운 방법을 찾아야 한다. 정말 좋은 서비스였는데 종료되어 아쉽다.


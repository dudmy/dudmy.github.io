---
layout: post
title: "Duplicate class found in modules"
categories: git
comments: true
---

의존성 충돌 문제가 발생했을 때를 위한 기록...

> Error: Duplicate class {clazz} found in modules {moduleA} and {moduleB}.

> Error: Conflict with dependency {dependency} in project ':app'.

Gradle을 사용하여 안드로이드 프로젝트의 dependency tree를 확인한다.

```
./gradlew app:dependencies
```

만약 출력을 텍스트 파일로 쓰고 싶다면 아래와 같이 입력한다.

```
./gradlew :app:dependencies > filename.txt
```

만약 HTML 파일 형태로 보고 싶다면 프로젝트 리포트 플러그인 추가 후 아래와 같이 입력한다.

```
apply plugin: 'project-report'
```

```
$ ./gradlew htmlDependencyReport
```

참고: [Using gradle to find dependency tree - Stack Overflow](https://stackoverflow.com/questions/21645071/using-gradle-to-find-dependency-tree)

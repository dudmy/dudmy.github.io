---
layout: post
title: "Kotlin 사용하기? unresolved reference error"
categories:
  - Android
comments: true
---

Dagger2 + DataBinding 을 이용하는 프로젝트에 Kotlin을 사용해보자!  
이를 위한 사전 작업인 플러그인은 설치되었다는 가정하에...  

　  

## 기본적인 코틀린 세팅  

참고: [Getting started with Android and Kotlin](http://kotlinlang.org/docs/tutorials/kotlin-android.html)

[build.gradle]  

```
buildscript {
    dependencies {
        // ...
        classpath 'org.jetbrains.kotlin:kotlin-gradle-plugin:1.1.1'
    }
}
```

[app/build.gradle]  

```
apply plugin: 'kotlin-android'

android {
    sourceSets {
        main.java.srcDirs += 'src/main/kotlin'
    }
}

dependencies {
    // ...
    compile 'org.jetbrains.kotlin:kotlin-stdlib:1.1.1'
    // ...
}
```
　

## Error: Unresolved reference: Dagger...Component  

여기까지 완료하고 빌드를 하면 위의 메시지와 함께 실패한다.  
이는 Dagger2의 자바 어노테이션 프로세싱을 지원하지 않아 생긴 문제로, 해결하기 위해서는 Kotlin Annotation Processing Tool 관련 코드를 추가해야 한다.  
참고: [kapt: Annotation Processing for Kotlin](https://blog.jetbrains.com/kotlin/2015/05/kapt-annotation-proㅋㅂcessing-for-kotlin/)  

[app/build.gradle]  

```
apply plugin: 'kotlin-kapt'

dependencies {
    // ...
    kapt 'com.google.dagger:dagger-compiler:2.8'
    // ...
}
```
　

## Error: Some error(s) occurred while processing annotations. Please see the error messages above.  

하지만 다시 빌드를 해도 위의 메시지와 함께 실패한다.  
이유는 첫 번째와 같고 이번에는 DataBinding 어노테이션으로 인해 발생한 문제이다. 아래의 코드를 추가하면 빌드 성공!

[app/build.gradle]  

```
dependencies {
    // ...
    kapt 'com.android.databinding:compiler:2.3.1'
    // ...
}
```


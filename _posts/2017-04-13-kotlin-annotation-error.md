---
layout: post
title: "Kotlin 사용하기? unresolved reference error"
excerpt: "Dagger2 + DataBinding 을 이용하는 프로젝트에 Kotlin을 사용? 적용? 해본다."
categories: [android]
comments: true
---

## 기본적인 코틀린 세팅  

참고: [Getting started with Android and Kotlin](http://kotlinlang.org/docs/tutorials/kotlin-android.html)

[build.gradle]  

```groovy
buildscript {
    dependencies {
        // ...
        classpath 'org.jetbrains.kotlin:kotlin-gradle-plugin:1.1.1'
    }
}
```

[app/build.gradle]  

```groovy
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

참고 링크
* [kapt: Annotation Processing for Kotlin](https://blog.jetbrains.com/kotlin/2015/05/kapt-annotation-processing-for-kotlin/)
* [Android Frameworks Using Annotation Processing](http://kotlinlang.org/docs/tutorials/android-frameworks.html)

[app/build.gradle]  

```groovy
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

```groovy
dependencies {
    // ...
    kapt 'com.android.databinding:compiler:2.3.1'
    // ...
}
```


## DataBinding error(s) ing... 

아직 이 문제에 대한 정확한 해결책은 아니지만, DataBinding 대신 [Kotlin Android Extensions](https://kotlinlang.org/docs/tutorials/android-plugin.html) 를 사용하는 것으로 문제를 피해가자!  

Extensions를 이용하면 별도의 뷰 인스턴스 선언 없이 id에 바로 접근하여 사용할 수 있다. 뷰의 id를 참조하게 되면 자동으로 import kotlinx.android.synthetic.{sourceSet}.{layout}.* 가 추가된다.  

이전에 추가했던 kapt 'com.android.databinding:compiler:2.3.1' 코드는 필요가 없어지니 삭제한다. 그리고 아래의 플러그인을 추가하면 사용할 수 있다.  

[app/build.gradle]  

```groovy
apply plugin: 'kotlin-android-extensions'
```



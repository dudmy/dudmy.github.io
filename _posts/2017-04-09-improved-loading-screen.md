---
layout: post
title: "개선된 로딩 화면 (Splash Screen)"
categories: android
comments: true
---

예전에 [안드로이드 로딩 화면 구현하기](http://dudmy.net/android/2015/08/11/create-android-loading/)에 대해 포스팅한 적이 있다. 같은 내용에 대해 다시 글을 작성하는 이유는 보다 나은 방법을 알게 되었기 때문이다.  


## 기존의 구현  

위의 포스팅을 확인해보면 알겠지만, 기존에 사용했던 방법은 핵심은 로딩 화면에서 [Handler.postDelayed](https://developer.android.com/reference/android/os/Handler.html#postDelayed(java.lang.Runnable, long))를 이용하여 일정 시간의 지연을 주는 것이다. 이러한 구현 방법에 대해 사용자의 입장과 개발자의 입장에서 다시 한번 고민할 필요가 있다.  

**1. 일정 시간의 기준?**  

로딩 화면 지연을 위한 '일정 시간'의 기준은 없다. 사용자는 로딩 화면이 띄어질 때 무엇을 하고 있는지 알 수 없으며 알고 싶지 않을 수도 있다. 즉, 앱을 실행할 때마다 '일정 시간'을 로딩 화면을 보면서 시간 낭비한다고 생각할 수도 있겠다.

![ils-1]({{ site.url }}/assets/post/20170409/ils-1.jpeg)  

**2. 안드로이드 앱의 Cold Start?**  

Cold start란 기기 부팅 후 앱이 처음 실행되거나 시스템에서 종료한 후에 앱을 시작하는 것을 말한다. 안드로이드는 특히 cold start 시동에 약간의 시간이 걸린다. 만약 기존의 방법으로 구현하는 경우, 아래의 이미지와 같이 빈 화면이 먼저 나타나고 나서야 로딩 화면이 보이게 된다.

![ils-2]({{ site.url }}/assets/post/20170409/ils-2.png)  


## 개선된 구현

개선된 구현의 핵심은 splash activity(로딩 화면)에 대한 layout 파일을 사용하지 않고 background theme를 이용하는 것이다.  

샘플 코드: [https://github.com/dudmy/blog-sample](https://github.com/dudmy/blog-sample/tree/master/Improved-Loading-Screen-Sample)  

* **splash_background.xml**  

우선 res/drawable 에 XML drawable 파일을 생성한다. 본 파일이 사용자에게 보이는 화면이다.  

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">

    <item android:drawable="@android:color/holo_blue_bright" />

    <item>
        <bitmap android:src="@mipmap/ic_launcher_round"
            android:gravity="center" />
    </item>

</layer-list>
```

* **values/styles.xml**  

다음으로는 splash activity의 background를 위한 theme를 만들어준다.  

```xml
<!-- Base application theme. -->
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar"> ... </style>

<!-- Splash theme. -->
<style name="SplashTheme" parent="Theme.AppCompat.NoActionBar">
    <item name="android:windowBackground">@drawable/splash_background</item>
</style>
```

* **SplashActivity.class**  

더는 기존에 사용되었던 Handler.postDelayed를 이용한 지연이 필요 없다. 그저 로딩 화면에서 해야할 작업들과 끝난 후의 처리만 해주면 된다. 

```java
public class SplashActivity extends AppCompatActivity {

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        Intent intent = new Intent(this, MainActivity.class);
        startActivity(intent);

        finish();
    }
}
```

* **AndroidManifest.xml**  

마지막으로 새로 만든 SplashTheme 스타일을 splash activity의 theme로 등록한다. Splash activity에 대한 layout 파일이 없으므로, 이에 대한 뷰는 theme에서 가져오게 된다.  

```xml
<activity android:name=".SplashActivity"
    android:theme="@style/SplashTheme">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
<activity android:name=".MainActivity" />
```

![ils-3]({{ site.url }}/assets/post/20170409/ils-3.jpeg)   

참고: [Splash Screens the Right Way](https://www.bignerdranch.com/blog/splash-screens-the-right-way/)

---
layout: post
title: "안드로이드 로딩 화면 구현하기"
categories:
  - Android
tags:
  - android
  - programming
  - loading
---

애플리케이션을 실행했을 때 로딩 화면 (인트로)이 나타나도록 구현한다. 예를 들면 카카오톡처럼 로고를 보여주는 것이다. 로딩 화면을 구현하는 방법은 두 가지로 나눌 수 있는데, 로딩 화면을 메인으로 설정하는 방법과 메인 화면에서 로딩 화면을 호출하는 방법이다.

전자는 앱의 시작점을 로딩 화면으로 설정하는 것으로 LoadingActivity → MainActivity 순서로 실행된다. 후자는 기본 시작점인 메인 화면에서 로딩 화면을 호출하는 것으로 MainActivity → LoadingActivity → MainActivity 순서로 실행된다.

방법의 차이일 뿐, 두 방법 모두 같은 결과물을 보여준다.  
　

## 로딩 화면을 메인으로 설정

* activity_loading.xml  

로딩 화면의 레이아웃을 간단하게 구성한다. xml을 만든 후 화면 중앙에 '로딩' 글자가 보이도록 TextView를 추가하였다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center_vertical">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="로딩"
        android:layout_gravity="center_horizontal"
        android:textSize="30dp" />

</LinearLayout>
```


* LoadingActivity.java  

activity_loading을 로딩 화면의 View로 설정한다. startLoading() 메소드는 로딩 화면이 나타난 후 2초뒤에 MainActivitiy를 실행하도록 한다.

```java
public class LoadingActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_loading);

        startLoading();
    }

    private void startLoading() {
        Handler handler = new Handler();
        handler.postDelayed(new Runnable() {
            @Override
            public void run() {
                Intent intent = new Intent(getBaseContext(), MainActivity.class);
                startActivity(intent);
                finish();
            }
        }, 2000);
    }

}
```

* AndroidManifest.xml  

애플리케이션의 시작점을 LoadingActivity로 변경한다. 그리고 아래에 MainActivity를 추가한다.

```xml
<activity
    android:name=".LoadingActivity"
    android:theme="@android:style/Theme.NoTitleBar.Fullscreen"
    android:label="@string/app_name" >
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
<activity android:name=".MainActivity" ></activity>
```
  
　

## 메인 화면에서 로딩 화면 호출

* LoadingActivity.java  

위와 다르게 MainActivity를 실행하지 않고 2초 뒤 종료된다.

```java
public class LoadingActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_loading);

        startLoading();
    }

    private void startLoading() {
        Handler handler = new Handler();
        handler.postDelayed(new Runnable() {
            @Override
            public void run() {
                finish();
            }
        }, 2000);
    }

}
```

* MainActivity.java  

onCreate() 부분에 LoadingActivity를 실행하는 코드를 추가한다.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    Intent intent = new Intent(this, LoadingActivity.class);
    startActivity(intent);
}
```

* AndroidManifest.xml  

처음 만들어진 상태 그대로 MainActivity가 시작점이다. 아래에 LoadingActivity에 대한 부분만 추가한다.

```xml
<activity
    android:name=".MainActivity"
    android:label="@string/app_name" >
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
<activity 
    android:name=".LoadingActivity"
    android:theme="@android:style/Theme.NoTitleBar.Fullscreen" >
</activity>
```

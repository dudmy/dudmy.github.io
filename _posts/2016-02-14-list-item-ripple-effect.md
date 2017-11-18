---
layout: post
title: "리스트 항목에 Ripple effect 적용"
excerpt:
categories: [android]
comments: true
---

리스트의 항목(Item) 터치 시 아무런 애니메이션 효과가 없을 때 Ripple effect 적용해본다. Ripple effect란 안드로이드 Material design의 기본 터치 피드백 애니메이션으로 물결 효과를 의미한다.

리스트의 항목 뷰 XML에 background를 지정하는 방식으로 기능을 적용해야 한다. 여기서는 일반적으로 사용되는 LinearLayout에 효과를 넣었다. 자세한 내용은 [Android Developers - Customize Touch Feedback](https://developer.android.com/training/material/animations.html) 에서 확인할 수 있다.

### 기본적인 제한된 물결 효과

```xml
<LinearLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="?android:attr/selectableItemBackground">
</LinearLayout>
```

![lire-1]({{ site.url }}/img/post/20160214/lire-1.png)

### 뷰를 넘어 확장되는 물결 효과

```xml
<LinearLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="?android:attr/selectableItemBackgroundBorderless">
</LinearLayout>
```

![lire-2]({{ site.url }}/img/post/20160214/lire-2.png)

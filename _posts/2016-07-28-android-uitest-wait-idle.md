---
layout: post
title: "안드로이드 UI 테스트에서 Idle 상태 기다리기"
categories: android
comments: true
---

안드로이드 UI 테스트를 하던 중 A 화면에서 버튼을 클릭했을 때 B 화면으로 넘어가는 동작이 있었다. 그러고 나서 B 화면에 알맞은 text가 보이는지 체크하는데 아래와 같은 오류가 떴다.

```
android.support.test.espresso.NoMatchingViewException: No views in hierarchy found matching: with ...
```

넘어가는 과정을 자세히 보면 A 화면 -> animation -> B 화면으로 진행된다. 짐작되는 원인으로는 animation이 동작하는 와중에 B 화면의 text를 찾아서 생긴 것으로 보인다. Espresso 입장에서는 아직 B 화면이 띄어지지 않았기 때문에, 당연히 매칭되는 뷰가 없다는 Exception을 던지는 것이다.

위 상황에 맞는 간단한 예제 테스트 코드를 작성해보면..

```java
@Test
public void testGoToNextPage() throws Exception {
    // Click button in page 'A'.
    onView(withId(R.id.button))
        .perform(click());

    // Check text is appered in page 'B'.
    onView(withText("B 화면"))
        .check(matches(isDisplayed()));
}
```

이 문제는 [UiDevice 클래스의 waitForIdle()](https://developer.android.com/reference/android/support/test/uiautomator/UiDevice.html#waitForIdle())을 이용하면 간단하게 해결 된다. waitForIdle()에 대한 설명을 보면 아래와 같이 나와 있다.

```
Waits for the current application to idle. Default wait timeout is 10 seconds.
```

즉, B 화면에 text가 보이는지 체크하기 전에 해당 메소드를 호출하면 된다.

```java
private UiDevice mDevice;

@Before
public void setUp() throws Exception {
    // Initialize UiDevice instance.
    mDevice = UiDevice.getInstance(getInstrumentation());
}

@Test
public void testGoToNextPage() throws Exception {
    // Click button in page 'A'.
    onView(withId(R.id.button))
        .perform(click());

    // Call method to wait before check.
    mDevice.waitForIdle();

    // Check text is appered in page 'B'.
    onView(withText("B 화면"))
        .check(matches(isDisplayed()));
}
```

UiDevice는 [Testing UI for Multiple Apps](https://developer.android.com/training/testing/ui-testing/uiautomator-testing.html)에서 사용되고 있는데, 나는 [Testing UI for a Single App](https://developer.android.com/training/testing/ui-testing/espresso-testing.html) 을 참고하여 Espresso testing framework로 테스트 코드를 짰다.

사실 아직 이 둘의 차이를 정확히 모르겠다. 그래서 이 방법이 정석적인? 방법이라고 말은 못하지만, 우선 원하는 대로 동작은 했으니... 추후에 일반적으로 사용되는 방법을 찾아봐야겠다. :)

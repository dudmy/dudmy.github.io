---
layout: post
title: "안드로이드 UI 테스트에서 Toast 체크하기"
categories:
  - Android
comments: true
---

얼마 전에 로그인 Process 관련하여 UI 테스트 코드를 짜게 되었다. Unit 테스트 코드는 짜봤지만, UI 테스트는 처음이라 대부분 새로운 내용이었고 하면서도 올바르게 짜고 있는지 의문이 많이 들었다.  

그래도 지난달 다녀온 [Google I/O 2016 Extended Seoul](https://festi.kr/festi/2016-io-extended-seoul-after/) 에서 정승욱 님의 [Advanced Espresso](http://www.slideshare.net/ssuser70b5b8/advanced-espresso-io16-extend-seoul) 발표를 듣고 많이 참고했다. 물론 봐도 적용을 못 한 게 대부분이지만 하하...  
　  

## Toast 체크하기

사용자가 어떠한 동작을 했을 때, 그에 맞는 올바른 Toast 메시지가 띄어졌는지 확인할 필요가 있다. 예를 들면 이메일, 비밀번호 등의 정보를 입력하고 로그인 버튼을 눌렀을 때, 잘못된 정보를 입력했으면 '입력하신 정보를 확인해주세요.' 라는 Toast가 떠야 할 것이다.

올바른 Toast 메시지가 떴는지 확인하는 코드는 [How to check Toast window, on android test-kit Espresso](http://baroqueworksdev.blogspot.kr/2015/03/how-to-check-toast-window-on-android.html)를 참고하였다.

```java
/**
 * Class to check toast message is shown.
 */
public class ToastMatcher extends TypeSafeMatcher<Root> {

    private String message;

    public ToastMatcher(String message) {
        this.message = message;
    }

    @Override
    protected boolean matchesSafely(Root root) {
        int type = root.getWindowLayoutParams().get().type;
        if (type == WindowManager.LayoutParams.TYPE_TOAST) {
            IBinder windowToken = root.getDecorView().getWindowToken();
            IBinder appToken = root.getDecorView().getApplicationWindowToken();
            if (windowToken == appToken) {
                // means this window isn't contained by any other windows.
                return true;
            }
        }
        return false;
    }

    @Override
    public void describeTo(Description description) {
        description.appendText(message);
    }

}
```

위의 ToastMatcher 코드를 Test class에 inner class 선언한다. 그리고 메시지를 체크하고 싶을 때에는 아래와 같이 적어주면 된다.

```java
onView(withText("입력하신 정보를 확인해주세요."))
    .inRoot(new ToastMatcher("입력하신 정보를 확인해주세요."))
    .check(matches(isDisplayed()));
```


---
layout: post
title: "안드로이드 Custom View 만들기 - 2"
categories: android
comments: true
---

이전 게시글에 이어서 [Android Training - Custom Views](https://developer.android.com/training/custom-views/index.html) 과정을 진행한다. 본 글은 발 번역 및 잘못된 생각이 포함될 수 있으니 참고하기만 바란다.

P.S. [GitHub Repo Issues](https://github.com/dudmy/Android-Training)


# Implementing Custom Drawing

Custom view에서 가장 중요한 부분은 외관이다. Custom 드로잉은 애플리케이션의 필요에 따라 쉽거나 복잡할 수 있다.  
이 레슨에서는 가장 일반적인 작업에 관해 설명한다.

## Override onDraw()

Custom view 그리기에서 가장 중요한 단계는 [onDraw()][onDraw] 메소드를 재정의 하는 것이다.  
[onDraw()][onDraw]의 파라미터는 뷰가 자신을 그릴 때 사용할 수 있는 [Canvas][canvas] 객체이다. [Canvas][canvas] 클래스는 텍스트, 선, 비트맵 및 다른 많은 그래픽 primitive를 그리는 방법을 정의한다. 이러한 [onDraw()][onDraw] 메소드를 사용하여 custom 사용자 인터페이스를 만들 수 있다. [#1][Override onDraw]

## Create Drawing Objects

[android.graphics](https://developer.android.com/reference/android/graphics/package-summary.html) 프레임워크는 두 가지 영역의 그리기로 나뉜다:

* [Canvas][canvas]에서 무엇을 그릴지를 처리
* [Paint][paint]에서 어떻게 그릴지를 처리

예를 들어, [Canvas][canvas]는 선을 그리는 메소드를 제공하고 [Paint][paint]는 선의 색상을 정의하는 메소드를 제공한다. [Canvas][canvas]는 사각형을 그릴 수 있는 메소드가 있고 [Paint][paint]는 사각형을 색상으로 채울지 또는 비워둘지 정의한다. 간단히 말해, [Canvas][canvas]는 화면에 그릴 수 있는 모양을 정의하고 [Paint][paint]는 그릴 모양의 색상, 스타일, 글꼴 등을 정의한다.

따라서, 무언가를 그리기 전에 하나 이상의 [Paint][paint] 객체를 만들어야 한다. PieChart 예제에서는 생성자에서 호출된 init이라고 불리는 메소드에서 이를 수행한다. [#1][Create Drawing Objects]

```java
private void init() {
    mTextPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
    mTextPaint.setColor(mTextColor);
    if (mTextHeight == 0) {
        mTextHeight = mTextPaint.getTextSize();
    } else {
        mTextPaint.setTextSize(mTextHeight);
    }

    mPiePaint = new Paint(Paint.ANTI_ALIAS_FLAG);
    mPiePaint.setStyle(Paint.Style.FILL);
    mPiePaint.setTextSize(mTextHeight);
    ...
}
```

객체를 미리 생성하는 것은 중요한 최적화이다. 뷰는 매우 빈번히 다시 그려지고, 많은 드로잉 객체들은 값비싼 초기화를 필요로 한다.  
[onDraw()][onDraw] 메소드 내에서 드로잉 객체를 만들면 성능이 크게 저하되고 UI가 느리게 보일 수 있다.

## Handle Layout Events

Custom view를 제대로 그리려면 그 크기를 알아야 한다. 복잡한 custom view는 종종 화면 영역의 크기와 모양에 따라서 다중 레이아웃 계산을 수행해야 한다. 절대로 화면에서 보이는 크기로 가정해서는 안 된다. 오직 하나의 앱에서 뷰를 사용하더라도, 앱은 세로 및 가로 모드에서 서로 다른 화면 크기, 다중 화면 밀도 그리고 다양한 종횡비를 처리해야 한다.

[View][view]에는 측정을 처리하는 다양한 방법이 있지만, 대부분은 재정의할 필요가 없다. 만약 뷰의 크기를 특별히 제어할 필요가 없으면 [onSizeChanged()][osc] 메소드 하나만 재정의하면 된다.

[onSizeChanged()][osc]는 뷰에 크기가 처음으로 할당될 때 호출되며, 어떤 이유로든 뷰의 크기가 변경되면 다시 호출된다. 그릴 때마다 다시 계산하는 대신에 [onSizeChanged()][osc]에서 뷰의 크기와 관련된 위치, 치수 및 기타값을 계산해야 한다.

뷰에 크기가 지정되면 레이아웃 매니저는 크기에 모든 패딩이 포함되어 있다고 가정한다. 따라서 뷰의 크기를 계산할 때 패딩값을 처리해야 한다. 아래는 PieChart.onSizeChanged()에서 이를 수행하는 방법이다.

```java
// Account for padding
float xpad = (float)(getPaddingLeft() + getPaddingRight());
float ypad = (float)(getPaddingTop() + getPaddingBottom());

// Account for the label
if (mShowText) xpad += mTextWidth;

float ww = (float)w - xpad;
float hh = (float)h - ypad;

// Figure out how big we can make the pie.
float diameter = Math.min(ww, hh);
```

만약 뷰의 레이아웃 파라미터를 보다 세밀하게 제어해야 한다면 [onMeasure()][om]를 구현하면 된다.  
이 메소드의 파라미터는 View.MeasureSpec 값으로, 뷰의 부모가 뷰를 얼마나 크게하고 싶은지 그리고 그 크기가 최댓값인지 또는 그저 권장값인지를 나타낸다. 최적화로서, 이 값은 압축된 정수로 저장되며 View.MeasureSpec의 static 메소드를 사용하여 각 정수에 저장된 정보를 풀어서 사용한다. [#1][Handle Layout Events]

```java
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
   // Try for a width based on our minimum
   int minw = getPaddingLeft() + getPaddingRight() + getSuggestedMinimumWidth();
   int w = resolveSizeAndState(minw, widthMeasureSpec, 1);

   // Whatever the width ends up being, ask for a height that would let the pie
   // get as big as it can
   int minh = MeasureSpec.getSize(w) - (int)mTextWidth + getPaddingBottom() + getPaddingTop();
   int h = resolveSizeAndState(MeasureSpec.getSize(w) - (int)mTextWidth, heightMeasureSpec, 0);

   setMeasuredDimension(w, h);
}
```

이 코드에는 세 가지 중요한 사항이 있다:

* 계산에는 뷰의 패딩이 고려된다. 앞서 언급했듯이, 이것은 뷰의 책임이다.
* Helper 메소드인 resolveSizeAndState()는 최종 너비와 높이를 만드는 데 사용된다. 이 helper는 뷰가 생각하는 크기를 [onMeasure()][om]에 전달된 스펙과 비교하여 적절한 View.MeasureSpec 값을 리턴한다.
* [onMeasure()][om]는 리턴값이 없다. 대신에, setMeasuredDimension()을 호출하여 결과를 전달한다. 이 메소드는 필수적으로 호출해야 한다. 만약 이 호출을 빼먹는다면 [View][view] 클래스가 런타임 예외를 발생시킨다.

> **Thinking...**  
> 일반적으로 뷰를 그리는데 중요한 세 가지 메소드로 onLayout(), onMeasure(), onDraw() 를 뽑는다. 어디에 위치할지 계산하고 어떻게 그릴지 측정하고 실제로 화면에 그리는 대부분 과정이 여기에 포함된다. 근데 막상 진행해보니 꼭 수학 문제를 푸는 것 같이 너무 복잡하다. 오픈 소스로 공개된 Custom View 코드들을 봐도 이해하기가 어렵다. 이것도 많이 구현해보는 게 답일까?

## Draw!

객체 생성 및 측정 코드를 정의한 후에는, [onDraw()][onDraw]를 구현할 수 있다.  
모든 뷰는 [onDraw()][onDraw]를 별도로 구현하지만, 대부분의 뷰에서는 몇 가지 공통된 작업이 있다. [#1][Draw]

* drawText()를 사용하여 텍스트를 그린다. setTypeface()를 호출하여 서체를 지정하고 setColor()를 호출하여 텍스트 색상을 지정한다.
* drawRect(), drawOval() 그리고 drawArc()를 사용하여 기본 도형을 그린다. setStyle()을 호출하여 도형의 채우기, 윤곽선 또는 두 가지 모두를 변경한다.
* Path 클래스를 사용하여 더욱 복잡한 모양을 그린다. Path 객체에 선과 곡선을 추가하여 모양을 정의한 다음, drawPath()를 사용하여 모양을 그린다. 기본 도형과 마찬가지로 setStyle()에 따라 경로를 채우기, 윤곽선 또는 두 가지 모두가 될 수 있다.
* LinearGradient 객체를 만들어 gradient 채우기를 정의한다. LinearGradient를 사용하여 모양을 채우려면 setShader()를 호출한다.
* drawBitmap()을 사용하여 비트맵을 그린다.


[onDraw]: https://developer.android.com/reference/android/view/View.html#onDraw(android.graphics.Canvas)
[canvas]: https://developer.android.com/reference/android/graphics/Canvas.html
[paint]: https://developer.android.com/reference/android/graphics/Paint.html
[view]: https://developer.android.com/reference/android/view/View.html
[osc]: https://developer.android.com/reference/android/view/View.html#onSizeChanged(int,%20int,%20int,%20int)
[om]: https://developer.android.com/reference/android/view/View.html#onMeasure(int,%20int)
[Override onDraw]: https://github.com/dudmy/Android-Training/commit/f1fd01de9088c6cc82a45c72fcbc73ab87e899d2
[Create Drawing Objects]: https://github.com/dudmy/Android-Training/commit/71d17a0bb922589dc43c989e6bb5d9a6f713298c
[Handle Layout Events]: https://github.com/dudmy/Android-Training/commit/58698d371c998c25e4d692f969000540263a81f8
[Draw]: https://github.com/dudmy/Android-Training/commit/a52728d55c75a3cde95cff382ee1f901c112caa6

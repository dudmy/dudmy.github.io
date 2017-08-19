---
layout: post
title: "안드로이드 Custom View 만들기 - 1"
categories:
  - Android
comments: true
---

Custom View를 만들어 본 경험이 있냐는 질문이 면접에서 빠지지 않는다는 것은 그만큼 실무에서 주로 사용된다는 뜻이다. 나는 아직 custom view 만들기를 아래의 코드와 같은 정사각형 이미지뷰 정도만 다루어보았다.

```java
public class SquareImageView extends AppCompatImageView {
    public SquareImageView(Context context) {
        super(context);
    }

    public SquareImageView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public SquareImageView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    @SuppressWarnings("SuspiciousNameCombination")
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, widthMeasureSpec);
    }
}
```

복잡한 custom view는 제대로 구현해 본 적이 없기 때문에 [Android Training](https://developer.android.com/training/custom-views/index.html)을 통해 학습해보려고 한다. 본 글은 발 번역 및 잘못된 생각(블럭 인용문에 적힌 내용)이 포함될 수 있으니 참고하기만 바란다.  
P.S. [GitHub Repo Issues](https://github.com/dudmy/Android-Training)

　  

## Summary

안드로이드 프레임 워크에는 사용자와 상호 작용하고 다양한 유형의 데이터를 표시하는데 필요한 [view][view] 클래스 집합이 있다. 그러나 때로는 기본 제공되는 뷰로 보완되지 않는 앱의 특별한 요구사항이 있다. 이 레슨은 강력하고 재사용 가능한 자체적인 뷰를 만드는 방법을 보여준다.

**Lesson**
* Creating a View Class
  - Android Studio 레이아웃 편집기의 지원 및 custom 속성들과 함께, 기본 제공되는 뷰처럼 동작하는 클래스를 만든다.
* Custom Drawing
  - 안드로이드 그래픽 시스템을 사용하여 시각적으로 눈에 띄는 뷰를 만든다.
* Making the View Interactive
  - 사용자는 뷰가 입력 제스처에 부드럽고 자연스럽게 반응할 것으로 기대한다. 이 레슨에서는 제스처 감지, 물리 및 애니메이션을 사용하여 사용자 인터페이스에 전문적인 느낌을 주는 방법에 관해 설명한다.
* Optimizing the View
  - UI가 얼마나 아름답든지 간에, 일관되게 높은 프레임 속도로 실행되지 않으면 사용자는 좋아하지 않을 것이다. 일반적인 성능 문제를 피하는 방법과 하드웨어 가속을 사용하여 custom 드로잉을 빠르게 실행하는 방법에 대해 알아본다.

　  

# Creating a Custom View Class

잘 디자인된 custom view는 다른 잘 디자인된 클래스와 매우 비슷하다. 사용하기 쉬운 인터페이스로 특정 기능 셋을 캡슐화하고, CPU 및 메모리를 효율적으로 사용하는 등이 있다. 잘 디자인된 클래스가 되는 것 외에도 custom view는 다음과 같아야 한다:

* 안드로이드 표준 준수
* 안드로이드 XML 레이아웃에서 작동하는 custom styleable 속성 제공
* 접근성 이벤트 보내기
* 여러 안드로이드 플랫폼과 호환되어야 한다

안드로이드 프레임 워크는 이러한 모든 요구 사항을 충족하는 뷰를 만드는 데 도움이 되는 기본 클래스와 XML 태그를 제공한다. 이 레슨에서는 안드로이드 프레임 워크를 사용하여 뷰 클래스의 핵심 기능을 만드는 방법에 관해 설명한다.

## Subclass a View

안드로이드 프레임 워크에 정의된 모든 뷰 클래스들은 [View][view]를 상속받는다. Custom view 또한 view를 직접 상속받을 수 있고, [Button](https://developer.android.com/reference/android/widget/Button.html)과 같은 view의 서브 클래스 중 하나를 상속받아 시간을 절약할 수도 있다. [#1][Subclass a View]

Android Studio와 뷰가 상호 작용하려면, 최소한 [Context](https://developer.android.com/reference/android/content/Context.html)와 [AttributeSet][attributeSet] 객체를 파라미터로 가지는 생성자를 제공해야 한다. 이 생성자는 레아이웃 편집기기가 뷰의 인스턴스를 만들고 편집할 수 있게 한다. [#2][Subclass a View]

```java
class PieChart extends View {
    public PieChart(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }
}
```

> ViewGroup
> 
> 바로 위의 예제에서 PieChart는 [View][view]를 상속받고 있다. 하지만 샘플 코드에서 PieChart는 [ViewGroup](https://developer.android.com/reference/android/view/ViewGroup.html)을 상속받고 있다. 그렇다면 이 두 클래스의 차이는 무엇일까?  
> 안드로이드 레퍼런스에 따르면 ViewGroup은 다른 뷰(자식뷰라고 불리는)를 포함할 수 있는 특수한 뷰로서 레이아웃 및 뷰 컨테이너의 기본 클래스이다. 즉, 뷰를 그룹으로 엮어서 관리하기 위하여 사용된다. 샘플 코드에서 PieChart는 실제로 표시되어야 하는 Pie 뷰와 Point 뷰를 포함하기 때문에 이를 상속받은 것으로 보인다.

## Define Custom Attributes

사용자 인터페이스에 기본 제공되는 [View][view]를 추가하려면, 이를 XML 요소에 지정하고 속성으로 모양과 동작을 제어해야 한다. 잘 작성된 custom view는 XML을 통해 값을 추가하고 스타일을 지정할 수 있다. custom view에서 이 동작을 사용하려면: [#1][Define Custom Attributes]

* < declare-styleable > 리소스 요소에서 뷰의 custom 속성을 정의
* XML 레이아웃의 속성값 지정
* 런타임 시 속성값 검색
* 검색된 속성값을 뷰에 적용

이 세션에서는 custom 속성을 정의하고 그 값을 지정하는 방법에 관해 설명한다. 다음 세션에서는 런타임 시에 값을 검색하고 적용하는 방법에 관해 설명한다.

Custom 속성을 정의하려면 < declare-styleable > 리소스를 프로젝트에 추가해야 한다. 이러한 리소스는 res/values/attrs.xml 파일에 저장하는 것이 일반적이다. 다음은 attrs.xml 파일의 예이다:

```xml
<resources>
   <declare-styleable name="PieChart">
       <attr name="showText" format="boolean" />
       <attr name="labelPosition" format="enum">
           <enum name="left" value="0"/>
           <enum name="right" value="1"/>
       </attr>
   </declare-styleable>
</resources>
```

이 코드는 PieChart라는 styleable entity에 속한 두 개의 custom 속성인, showText와 labelPosition을 선언한다. Styleable entity의 이름은 일반적으로 custom view를 정의하는 클래스 이름과 같다.

> Got to Know
> 
> LinearLayout 속성의 구성이나 구조에 대해 알게 되었다. XML attributes 중에서 clickable은 boolean 포맷이고 visibility는 enum 포맷이며 constant(value)로 visible(0), invisible(1), gone(2)을 가진다는 것을 이해했다.

Custom 속성을 정의한 후에는 기본 제공되는 속성처럼 레이아웃 XML 파일에서 사용할 수 있다. 유일한 차이점은 custom 속성이 다른 namespace에 속한다는 것이다. 즉, 예제에서는 'http://schemas.android.com/apk/res/android' 대신에 'http://schemas.android.com/apk/res/[your package name]'에 속한다. 

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:custom="http://schemas.android.com/apk/res/net.dudmy.customviews">
    <net.dudmy.customviews.PieChart
        custom:showText="true"
        custom:labelPosition="right" />
</LinearLayout>
```

긴 namespace URI를 반복하지 않도록 샘플에서는 xmlns 지시문을 사용한다. 이 지시문은 'http://schemas.android.com/apk/res/net.dudmy.customviews' namespace를 별칭 custom에 할당한다. Namespace에 대해 원하는 별칭은 선택할 수 있다.

> 참고로, custom attributes 사용을 위해 추가하는 namespace를 'http://schemas.android.com/apk/res/[your package name]' 대신 'http://schemas.android.com/apk/res-auto'를 사용해도 된다. [#2][Define Custom Attributes]

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <net.dudmy.customviews.PieChart
        app:showText="true"
        app:labelPosition="right" />
</LinearLayout>
```

## Apply Custom Attributes

뷰가 XML 레이아웃으로부터 생성되었을 때, XML 태그 내의 모든 속성은 resource bundle로부터 읽혀 뷰 생성자의 [AttributeSet][attributeSet]로서 전달된다. [AttributeSet][attributeSet]에서 직접 값을 읽을 수도 있지만, 이는 몇 가지 단점이 있다:

* 속성값 내의 resource 참조가 해결되지 않는다
* 스타일이 적용되지 않는다

대신에, [obtainStyledAttributes()][obtainStyledAttributes]으로 [AttributeSet][attributeSet]을 전달하면 된다. 이 메소드는 이미 참조 해제되고 스타일 지정된 [TypedArray][typedArray] 값 배열을 다시 전달한다. [#1][Apply Custom Attributes]

안드로이드 리소스 컴파일러는 [obtainStyledAttributes()][obtainStyledAttributes]를 보다 쉽게 호출할 수 있도록 많은 작업을 수행한다. res 디렉터리의 각 < declare-styleable > 리소스에 대해 생성된 R.java는 배열의 속성 id와 배열의 각 속성에 대한 인덱스를 정의하는 상수 셋을 모두 정의한다. 미리 정의된 상수를 사용하여 [TypedArray][typedArray]에서 특성을 읽는다. PieChart 클래스에서 속성을 읽는 방법은 다음과 같다:

```java
public PieChart(Context context, AttributeSet attrs) {
    super(context, attrs);
    TypedArray a = context.getTheme().obtainStyledAttributes(
        attrs,
        R.styleable.PieChart,
        0, 0);

    try {
      mShowText = a.getBoolean(R.styleable.PieChart_showText, false);
      mTextPos = a.getInteger(R.styleable.PieChart_labelPosition, 0);
    } finally {
      a.recycle();
    }
}
```

> 주의사항
> 
> [TypedArray][typedArray] 객체는 공유 resource이므로 사용 후 반드시 재활용해야 한다. 이를 위해 try-finally에서 recycle()을 호출한다. 할당되어 있던 메모리를 풀에 즉시 돌려줘서 GC 될 때까지 기다릴 필요가 없게 된다. [#2][Apply Custom Attributes]

## Add Properties and Events

Attributes는 동작 및 모양을 제어하는 강력한 방법이지만, 뷰가 초기화되었을 때만 읽을 수 있다. 동적인 동작을 제공하려면, 각 custom attribute에 대한 getter와 setter 쌍을 공개해야 한다. [#1][Add Properties and Events]

```java
public boolean isShowText() {
   return mShowText;
}

public void setShowText(boolean showText) {
   this.mShowText = showText;
   invalidate();
   requestLayout();
}
```

setter에서 [invalidate()](https://developer.android.com/reference/android/view/View.html#invalidate())와 [requestLayout()](https://developer.android.com/reference/android/view/View.html#requestLayout())을 부르는 것은 뷰가 안정적으로 작동하는지 확인하는 데 중요하다. 외형에 영향을 주는 속성을 변경한 경우, 뷰를 무효화해야 다시 그려야 한다는 것을 시스템이 알 수 있다. 마찬가지로, 크기나 모양에 영향을 주는 속성이 변경된 경우 새로운 레이아웃을 요청해야 한다. 이러한 메소드 호출을 잊어버리면 찾기 힘든 버그가 발생할 수 있다. [#2][Add Properties and Events]

> Invalidate?
> 
> 이 단어의 사전적 의미는 '무효화하다'이다. 즉, invalidate()를 호출하면 해당 뷰 전체가 무효화된다. 만약 뷰가 visible 상태이면, 미래의 어느 시점에서 onDraw()가 호출될 것이다.  
> 주의사항으로 이는 반드시 UI 스레드에서 호출해야 한다. Non-UI 스레드에서 호출하려면 postInvalidate()를 이용하면 된다.

또한, custom view는 중요한 이벤트를 전달하기 위한 이벤트 리스너를 지원해야 한다. 예를 들어, PieChart에서는 OnCurrentItemChanged 라는 이벤트를 공개하여 사용자가 pie chart를 회전하여 새로운 pie 조각에 포커스를 맞춘 것을 리스너에게 알려야 한다. [#3][Add Properties and Events]

당신이 custom view의 유일한 사용자인 경우 속성 및 이벤트를 공개하는 것을 잊어버리기 쉽다. 뷰의 인터페이스를 신중하게 정의하는 데 시간을 할애하면 향후 유지 관리하는데 비용을 줄여준다. 따라야 할 좋은 규칙은 custom view의 표시되는 모양이나 동작에 영향을 주는 속성을 항상 공개하는 것이다.

## Design For Accessibility

Custom view는 넓은 범위의 사용자를 고려하여 지원해야 한다. 여기에는 터치스크린을 보지 못하거나 사용하지 못하는 사용자들이 포함된다. 이러한 사용자들을 지원하라면 다음을 수행해야 한다:

* android:contentDescription 속성을 사용하여 입력란에 라벨을 지정
* 적절한 경우 [sendAccessibilityEvent()](https://developer.android.com/reference/android/view/accessibility/AccessibilityEventSource.html#sendAccessibilityEvent(int))를 호출하여 접근성 이벤트 보내기
* D-pad 및 트랙볼과 같은 대체 컨트롤러를 지원



[view]: https://developer.android.com/reference/android/view/View.html
[attributeSet]: https://developer.android.com/reference/android/util/AttributeSet.html
[obtainStyledAttributes]: https://developer.android.com/reference/android/content/res/Resources.Theme.html#obtainStyledAttributes(android.util.AttributeSet,%20int[],%20int,%20int)
[typedArray]: https://developer.android.com/reference/android/content/res/TypedArray.html
[Subclass a View]: https://github.com/dudmy/Android-Training/commit/bd50d7ddd1d390dd3fb74ba77aa0c9793a3e958b
[Define Custom Attributes]: https://github.com/dudmy/Android-Training/commit/3df954ffd60585f5d13b8112348827fbc3d6201c
[Apply Custom Attributes]: https://github.com/dudmy/Android-Training/commit/5e5a10a71e0c54df617115fef8e6295d7e7d83d6
[Add Properties and Events]: https://github.com/dudmy/Android-Training/commit/a667e10926ad34b039b63955cf71b5d53c06e675

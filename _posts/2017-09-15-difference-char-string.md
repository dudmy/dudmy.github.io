---
layout: post
title: "[얕지깊파] CharSequence와 String의 차이"
categories:
  - Android
comments: true
---

그동안 아무 생각 없이 사용했지만, 개념에 대해 고민하지 않았던 내용을 정리해보려고 한다. 일명, 얕은 지식 깊게 파기. 첫 번째 순서로 안드로이드에서 텍스트를 다룰 때 주로 사용하는 [CharSequence][char]와 [String][str]의 차이에 대해 알아본다.


## String?

[String][str]은 하나의 클래스이다. Java 프로그램의 모든 문자열 리터럴은 이 클래스의 인스턴스로 구현된다. 중요한 점은 문자열 값은 작성된 후에 변경할 수 없다는 것이다. [String][str] 객체에 보관하는 문자열은 유니코드로 변형되므로 HTML과 같은 마크업 문자를 입출력할 때 문제가 발생한다. 이와 같이 마크업 문자를 입력하여 사용할 수 없기 때문에 변경할 수 없는 문자열, 불변 문자열이라고 부른다. 

추가로, 변경 가능한 문자열을 지원하는 [StringBuilder](https://developer.android.com/reference/java/lang/StringBuilder.html)와 [StringBuffer](https://developer.android.com/reference/java/lang/StringBuffer.html)가 있는데 이는 나중에 다루어보도록 한다.

> 유니코드란?
> 위키백과에 따르면 유니코드(Unicode)는 전 세계의 모든 문자를 컴퓨터에서 일관되게 표현하고 다룰 수 있도록 설계된 산업 표준이다. 유니코드의 목적은 현존하는 문자 인코딩 방법을 모두 유니코드로 교체하려는 것이다.

> 마크업 문자란?
> 위키백과에 따르면 마크업 언어(markup 言語, markup language)는 태그 등을 이용하여 문서나 데이터의 구조를 명기하는 언어의 한 가지이다. 대표적인 예로, HTML 마크업을 사용하여 문자열에 스타일을 추가할 수 있고 안드로이드에서는 태그를 사용하여 구조적인 데이터를 나타내는 XML 마크업을 활용할 수 있다.

## CharSequence?

[CharSequence][char]는 클래스가 아니라 인터페이스이다. 인터페이스명(character + sequence)에서 짐작되듯이 char 값을 읽을 수 있는 시퀀스이다. 그리고 이 인터페이스는 다양한 종류의 char 시퀀스에 대해 균일한 읽기 전용 접근 권한을 제공한다. [CharSequence][char]를 implements 하여 구현된 대표적인 클래스로는 String, SpannableStringBuilder, StringBuilder, StringBuffer 등이 있다.

중요한 점은 [CharSequence][char] 객체에 보관하는 문자열은 같은 유니코드라도 마크업 문자를 사용할 수 있다. 이처럼 [String][str] 클래스와 반대로 변형과 가공을 할 수 있기 때문에 스타일 문자 또는 연속된 문자라고도 한다. 아래와 같은 XML에서 사용하는 스타일이다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="hello">Hello, <b>world</b>!</string>
</resources>
```

## Example

위에서 설명한 [CharSequence][char]와 [String][str]의 차이를 SpannableStringBuilder 예제를 통해서 알아보고자 한다. 그 전에 예제에 사용되는 클래스에 대해 간략히 설명하자면, [Spannable](https://developer.android.com/reference/android/text/Spannable.html)은 마크업 객체를 붙이고 분리할 수 있는 텍스트를 위한 인터페이스이다. 그리고 이를 implements 하여 구현된 클래스가 SpannableStringBuilder 이다. 즉, SpannableStringBuilder는 색상 및 글꼴 두께와 같은 서체 속성을 사용하여 스타일을 지정할 수 있는 텍스트 객체이다.

샘플 코드: [https://github.com/dudmy/blog-sample](https://github.com/dudmy/blog-sample/tree/master/20170915-difference-char-string) 

```java
public CharSequence getTextWithIcon(String text, Drawable icon) {
    ImageSpan imageSpan = new ImageSpan(icon);
    SpannableStringBuilder builder = new SpannableStringBuilder();
    builder.append("#") // (1)
            .append(text) // (2)
            .setSpan(imageSpan, 0, 1, Spanned.SPAN_INCLUSIVE_EXCLUSIVE); // (3)
    return builder;
}

public class SectionsPagerAdapter extends FragmentPagerAdapter {
    // ...
    @Override
    public CharSequence getPageTitle(int position) {
        Drawable icon = getDrawable(R.mipmap.ic_launcher_round);
        CharSequence title = getTextWithIcon("HELLO", icon);
        return position == 0 ? title : title.toString(); // (4)
    }
}
```

위의 getTextWithIcon() 메소드는 text와 icon을 파라미터로 받아 icon + text 형태를 반환하도록 구현한 것이다. 만약 파라미터의 값이 "HELLO"와 😀 이라면, 아래의 과정처럼 진행될 것이고 앞서 알아보았던 [CharSequence][char]와 [String][str]의 차이를 확인할 수 있다.

(1) "#" 추가한다. -> "#"  
(2) text 값을 덧붙인다. -> "#HELLO"  
(3) [start, end] = [0, 1] 영역을 ImageSpan으로 세팅한다. -> "😀HELLO"  
(4) position == 0 : 타이틀이 [CharSequence][char]로 반환되므로 "😀HELLO"로 표시된다.  
position != 0 : 타이틀이 [String][str]으로 변환된 후 반환되므로 "#HELLO"로 표시된다.


[char]: https://developer.android.com/reference/java/lang/CharSequence.html
[str]: https://developer.android.com/reference/java/lang/String.html

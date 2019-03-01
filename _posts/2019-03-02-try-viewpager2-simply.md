---
layout: post
title: "ViewPager2 간단히 사용해보기"
categories: android
comments: true
---

안드로이드 [ViewPager2][vp2]의 알파 버전이 2019년 2월 7일에 릴리즈되었다. 새로운 기능으로는 RTL 레이아웃과 수직 스크롤링이 지원되고 기존 [ViewPager][vp1] 버그 수정으로 `notifyDataSetChanged` 기능이 완전히 동작한다.  

샘플 코드 : [https://github.com/dudmy/blog-sample](https://github.com/dudmy/blog-sample/tree/master/ViewPager2-Sample)  

---

우선 build.gradle에 의존성을 추가한다. [ViewPager2][vp2]는 Android X 용으로 출시되어서 사용하려면 프로젝트가 Android X로 마이그레이션 되어야 한다.

```
implementation 'androidx.viewpager2:viewpager2:1.0.0-alpha01'
```

Activity 또는 Fragment에 [ViewPager2][vp2] 위젯을 추가한다.

```xml
<androidx.viewpager2.widget.ViewPager2
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

## RecyclerView.Adapter

[ViewPager2][vp2]에서는 RecyclerView.Adapter가 PagerAdapter를 대체하게 되었다. RecyclerView에서의 사용 방법과 같기 때문에 별도의 학습비용이 들지 않는다.

```kotlin
class PagerRecyclerAdapter(private val bgColors: List<Int>) : RecyclerView.Adapter<PagerViewHolder>() {

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): PagerViewHolder =
            PagerViewHolder(LayoutInflater.from(parent.context).inflate(R.layout.item_pager, parent, false))

    override fun onBindViewHolder(holder: PagerViewHolder, position: Int) {
        holder.bind(bgColors[position], position)
    }

    override fun getItemCount(): Int = bgColors.size
}

class PagerViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {

    private val textView: TextView = itemView.findViewById(R.id.page_name)

    fun bind(@ColorRes bgColor: Int, position: Int) {
        textView.text = "RecyclerViewAdapter\nPage $position"
        itemView.setBackgroundColor(ContextCompat.getColor(itemView.context, bgColor))
    }
}
```

마지막으로 [ViewPager2][vp2]에 어댑터를 설정해주면 된다.

```kotlin
pager.adapter = PagerRecyclerAdapter(bgColors)
pager.orientation = ViewPager2.ORIENTATION_HORIZONTAL
```

![img-1]({{ site.url }}/assets/post/20190302/img-1.gif)

## FragmentStateAdapter

FragmentStateAdapter를 이용하면 기존 [ViewPager][vp1]처럼 fragment로 구성할 수 있다. 기존 API인 FragmentStatePagerAdapter를 대체한 어댑터이다.

```kotlin
class PagerFragmentStateAdapter(private val bgColors: List<Int>, fm: FragmentManager) : FragmentStateAdapter(fm) {

    override fun getItem(position: Int): Fragment =
            PagerFragment.newInstance(bgColors[position], position)

    override fun getItemCount(): Int = bgColors.size
}

class PagerFragment : Fragment() {

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? =
            inflater.inflate(R.layout.item_pager, container, false)

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        arguments?.let {
            page_name.text = "FragmentStateAdapter Page ${it.getInt("position")}"
            view.setBackgroundColor(ContextCompat.getColor(view.context, it.getInt("bgColor")))
        }
    }

    companion object {
        fun newInstance(@ColorRes bgColor: Int, position: Int): PagerFragment {
            val bundle = Bundle()
            bundle.putInt("bgColor", bgColor)
            bundle.putInt("position", position)
            return PagerFragment().apply { arguments = bundle }
        }
    }
}
```

이번에는 수직 스크롤링으로 설정해보았다.

```kotlin
pager.adapter = PagerFragmentStateAdapter(bgColors, supportFragmentManager)
pager.orientation = ViewPager2.ORIENTATION_VERTICAL
```

![img-2]({{ site.url }}/assets/post/20190302/img-2.gif)

## OnPageChangeCallback

기존 [ViewPager][vp1]의 addPageChangeListener는 인터페이스여서 메서드를 모두 재정의해야 했다. 하지만 [ViewPager2][vp2]의 OnPageChangeCallback은 추상 클래스이기 때문에 필요한 메서드만 재정의하면 된다.

```kotlin
pager.registerOnPageChangeCallback(object : ViewPager2.OnPageChangeCallback() {
    override fun onPageSelected(position: Int) {
        super.onPageSelected(position)
    }
})
```

이제 첫 번째 릴리즈라서 Known issues가 좀 있지만, 기존 문제점이 많이 보완되고 사용법이 쉽기 때문에 [ViewPager2][vp2]가 [ViewPager][vp1]를 충분히 대체할 수 있을 것 같다.

참고 자료)  
* [ViewPager2 Releases - Android Developers](https://developer.android.com/jetpack/androidx/releases/viewpager2?hl=ko)
* [ViewPager2 Reference - Android Developers](https://developer.android.com/reference/androidx/viewpager2/widget/ViewPager2)


[vp1]: https://developer.android.com/reference/android/support/v4/view/ViewPager
[vp2]: https://developer.android.com/reference/androidx/viewpager2/widget/ViewPager2
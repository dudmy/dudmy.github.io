---
layout: post
title: "Drag and Swipe RecyclerView"
categories: android
comments: true
---

RecyclerView에서 아이템을 편집할 때 사용하는 대표적인 제스처 2가지가 있다.

- Drag & Drop : 아이템 순서 바꾸기
- Swipe to Dismiss : 아이템 제거하기

Android Support Library에는 RecyclerView에 위 기능 추가를 지원하는 유틸리티 클래스가 포함되어 있다.

## ItemTouchHelper

[ItemTouchHelper][ith]는 RecyclerView.ItemDecoration의 서브 클래스이다. RecyclerView 및 Callback 클래스와 함께 작동하며, 사용자가 이러한 액션을 수행할 때 이벤트를 수신한다. 우리는 지원하는 기능에 따라 메서드를 재정의해서 사용하면 된다.

[ItemTouchHelper.Callback][cb]은 추상 클래스로 추상 메서드인 getMovementFlags(), onMove(), onSwiped()를 필수로 재정의해야 한다. 아니면 Wrapper 클래스인 [ItemTouchHelper.SimpleCallback](https://developer.android.com/reference/android/support/v7/widget/helper/ItemTouchHelper.SimpleCallback)을 이용해도 된다.

동작에 대한 애니메이션도 추가할 수 있지만, 여기서는 간단하게 구현만 해본다.

샘플 코드 : [https://github.com/dudmy/blog-sample](https://github.com/dudmy/blog-sample/tree/master/ItemTouchHelper-Sample)

#### item_artist.xml

뷰홀더의 레이아웃은 아래와 같이 구성한다. 아이템 정보들과 오른쪽에 드래그 핸들 아이콘을 추가했다.

![img-1]({{ site.url }}/assets/post/20180502/img-1.png)

#### ItemDragListener.kt

사용자가 Drag 액션을 시작할 때 itemTouchHelper에 이벤트를 전달한다.

```kotlin
interface ItemDragListener {
    fun onStartDrag(viewHolder: RecyclerView.ViewHolder)
}
```

#### ItemActionListener.kt

아이템이 Drag & Drop 됐거나 Swiped 됐을 때 어댑터에 이벤트를 전달한다.

```kotlin
interface ItemActionListener {
    fun onItemMoved(from: Int, to: Int)
    fun onItemSwiped(position: Int)
}
```

#### MainAdapter.kt

어댑터에서는 ItemActionListener 인터페이스를 구현한다. onItemMoved(), onItemSwiped()을 재정의하여 아이템 이동과 제거 코드를 작성한다. 이때 어댑터가 아이템 변경 사항을 인식할 수 있도록 notifyItemMoved(), notifyItemRemoved()를 호출해야 한다.

```kotlin
class MainAdapter(
        private val items: MutableList<Artist>,
        private val listener: ItemDragListener
) : RecyclerView.Adapter<MainAdapter.ViewHolder>(), ItemActionListener {
    // ...

    override fun onItemMoved(from: Int, to: Int) {
        if (from == to) {
            return
        }

        val fromItem = items.removeAt(from)
        items.add(to, fromItem)
        notifyItemMoved(from, to)
    }

    override fun onItemSwiped(position: Int) {
        items.removeAt(position)
        notifyItemRemoved(position)
    }
}
```

#### MainAdapter.ViewHolder

어댑터 생성자의 파라미터로 받은 ItemDragListener는 뷰홀더에서 사용된다. 여기서는 드래그 핸들을 통한 아이템 이동을 구현하고자 하기 때문에, 드래그 핸들 뷰에 터치 리스너를 달아준다. 그리고 사용자가 [ACTION_DOWN](https://developer.android.com/reference/android/view/MotionEvent.html#ACTION_DOWN) 액션을 취했을 때 listener.onStartDrag()를 호출한다.

```kotlin
class ViewHolder(itemView: View, listener: ItemDragListener) : RecyclerView.ViewHolder(itemView) {

    init {
        itemView.drag_handle.setOnTouchListener { v, event ->
            if (event.action == MotionEvent.ACTION_DOWN) {
                listener.onStartDrag(this)
            }
            false
        }
    }
    // ...
}
```

#### ItemTouchHelperCallback.kt

[ItemTouchHelper.Callback][cb]을 상속받는 ItemTouchHelperCallback 클래스를 구현한다. 생성자의 파라미터로 ItemActionListener를 받는다.

1. 우선 getMovementFlags()를 재정의해 Drag 및 Swipe 이벤트의 방향을 지정한다.
2. 아이템이 Drag 되면 [ItemTouchHelper][ith]는 onMove()를 호출한다. 이때 ItemActionListener로 어댑터에 fromPosition과 toPosition을 파라미터와 함께 콜백을 전달한다.
3. 아이템이 Swipe 되면 [ItemTouchHelper][ith]는 범위를 벗어날 때까지 애니메이션을 적용한 후 onSwiped()를 호출한다. 이때 ItemActionListener로 어댑터에 제거할 아이템의 position을 파라미터와 함께 콜백을 전달한다.
4. isLongPressDragEnabled()은 아이템을 길게 누르면 Drag & Drop 작업을 시작해야 하는지를 반환한다. 디폴트는 true이지만, 여기서는 불필요하기 때문에 재정의하여 동작을 비활성화한다.

```kotlin
class ItemTouchHelperCallback(val listener: ItemActionListener) : ItemTouchHelper.Callback() {

    override fun getMovementFlags(recyclerView: RecyclerView?, viewHolder: RecyclerView.ViewHolder?): Int {
        val dragFlags = ItemTouchHelper.DOWN or ItemTouchHelper.UP
        val swipeFlags = ItemTouchHelper.START or ItemTouchHelper.END
        return makeMovementFlags(dragFlags, swipeFlags)
    }

    override fun onMove(recyclerView: RecyclerView?, viewHolder: RecyclerView.ViewHolder?, target: RecyclerView.ViewHolder?): Boolean {
        listener.onItemMoved(viewHolder!!.adapterPosition, target!!.adapterPosition)
        return true
    }

    override fun onSwiped(viewHolder: RecyclerView.ViewHolder?, direction: Int) {
        listener.onItemSwiped(viewHolder!!.adapterPosition)
    }

    override fun isLongPressDragEnabled(): Boolean = false
}
```

#### MainActivity.kt

액티비티에서는 ItemDragListener 인터페이스를 구현한다. 뷰홀더에서 onStartDrag() 이벤트를 보내면 [ItemTouchHelper.startDrag()](https://developer.android.com/reference/android/support/v7/widget/helper/ItemTouchHelper.html#startDrag(android.support.v7.widget.RecyclerView.ViewHolder)) 메서드를 호출하여 파라미터로 전달된 뷰홀더 Drag를 시작한다.
마지막으로 onCreate()에서 ItemTouchHelperCallback을 파라미터로 하는 [ItemTouchHelper][ith]를 생성하고 RecyclerView에 붙여주면 된다.

```kotlin
class MainActivity : AppCompatActivity(), ItemDragListener {
    // ...

    override fun onCreate(savedInstanceState: Bundle?) {
        // ...

        itemTouchHelper = ItemTouchHelper(ItemTouchHelperCallback(mainAdapter))
        itemTouchHelper.attachToRecyclerView(recycler_view)
    }

    override fun onStartDrag(viewHolder: RecyclerView.ViewHolder) {
        itemTouchHelper.startDrag(viewHolder)
    }
}
```

## Sample

* Drag & Drop

![img-2]({{ site.url }}/assets/post/20180502/img-2.gif)

* Swipe to Dismiss

![img-3]({{ site.url }}/assets/post/20180502/img-3.gif)



[ith]: https://developer.android.com/reference/android/support/v7/widget/helper/ItemTouchHelper
[cb]: https://developer.android.com/reference/android/support/v7/widget/helper/ItemTouchHelper.Callback

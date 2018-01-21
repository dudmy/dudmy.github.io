---
layout: post
title: "RecyclerView에 대한 고찰"
categories: android
comments: true
---

## ListView

Android 4.4(Kitkat) 까지는 아이템 목록을 보여주기 위해 [ListView](https://developer.android.com/reference/android/widget/ListView.html?hl=ko) 위젯이 사용되었다. 그리고 더 좋은 성능(ex. smooth scroll)을 위해 사용 방법이 변해왔다.

```java
public class MyAdapter extends BaseAdapter {
    // override other abstract methods here

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View view = getLayoutInflater().inflate(R.layout.list_itme, parent, false);
        
        ((TextView) view.findViewById(R.id.text_view))
                .setText("sample text");
        return view;
    }
}
```

위와 같이 Adapter를 작성하면 기대하던 대로 보이지만 ListView의 특성을 제대로 활용하지 못했다. getView()는 새로운 아이템을 표시할 때마다 호출되는데, 매번 View를 inflate 하고 있으므로 불필요한 메모리가 낭비되며 성능상에도 문제가 생긴다. 이러한 문제를 해결하기 위해 ListView는 화면에 보이는 아이템만큼의 View를 재활용하는 구조로 되어있다.

### View의 재활용

```java
public class MyAdapter extends BaseAdapter {
    // override other abstract methods here

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        if (convertView == null) {
            convertView = getLayoutInflater().inflate(R.layout.list_itme, parent, false);
        }
        
        ((TextView) convertView.findViewById(R.id.text_view))
                .setText("sample text");
        return convertView;
    }
}
```

getView()의 파라미터로는 아이템의 위치인 position, 재활용 뷰인 convertView, 그리고 부모 뷰인 parent가 전달된다. 즉, 재활용 뷰를 활용하여 convertView가 null이 아닌 경우에는 그대로 이용하면 되고 null인 경우에만 inflate 하면 된다. Android Reference에도 이와 같은 방법으로 예제가 주어져 있다.

### ViewHolder 패턴

여기서 조금 더 나은 성능을 위해서 어떤 작업을 하면 좋을까? 위 코드를 보면 View 자체는 재활용하는데 정작 View가 가진 TextView는 매번 findViewById()로 얻어오고 있다. 예제에는 TextView 하나만 있지만, 일반적으로 View의 구조는 복잡하므로 성능상에 문제가 될 수 있다.

```java
public class MyAdapter extends BaseAdapter {
    // override other abstract methods here

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        ViewHolder holder;

        if (convertView == null) {
            convertView = getLayoutInflater().inflate(R.layout.list_itme, parent, false);
            holder = new ViewHolder();
            holder.textView = (TextView) convertView.findViewById(R.id.text_view);
            convertView.setTag(holder);
        } else {
            holder = (ViewHolder) convertView.getTag();
        }

        holder.textView.setText("sample text");
        return convertView;
    }

    private static class ViewHolder {
        TextView textView;
    }
}
```

이러한 문제를 해결하기 위해 View의 정보를 들고 있을 ViewHolder라는 클래스를 만들어서 사용한다. convertView가 null이면 View를 inflate 한 후에 findViewById()로 얻은 View를 ViewHolder에 세팅하고 이를 convertView의 Tag에 저장한다. 그러면 재활용 뷰가 존재하는 경우에는 불필요한 findViewById() 과정 없이 아이템을 보여줄 수 있게 된다.


## RecyclerView

[RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html) 위젯은 Android 5.0(Lollipop)에서 처음 소개되었다. Android Developers 사이트에 나온 설명을 인용하면, RecyclerView는 ListView의 더욱 향상되고 유연해진 버전이다. 이는 한정된 수의 뷰를 유지함으로써 매우 효율적으로 스크롤 할 수 있는 큰 데이터 집합을 표시하기 위한 컨테이너이다.

### ViewHolder 패턴의 강제화

앞서 살펴봤듯이 ListView는 재활용 뷰를 이용하는 것까지가 기본 예제이다. 따라서 보다 나은 성능을 고려하지 않거나 ViewHolder 패턴에 대해 접해보지 않으면 적용하기 어렵다. 또한, 이와 같은 형태를 매번 직접 구현하는 것은 귀찮다. 이러한 이유로 RecyclerView에서는 ViewHolder 패턴을 강제화하여 구현하게 되어있는 것 같다.

```java
/**
 * RecyclerView.Adapter를 상속받을 때, Adapter에 대한 ViewHolder를
 * 반드시 구현해야 하고 이를 명시해주어야 한다.
 */
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder> {
    // override other abstract methods here

    static class ViewHolder extends RecyclerView.ViewHolder {
        private TextView textView;

        ViewHolder(View itemView) {
            super(itemView);
            textView = (TextView) itemView.findViewById(R.id.text_view);
        }
    }
}
```

### 여러 타입의 뷰

아이템 목록을 보여줄 때 한 가지 타입의 뷰만 보여줄 수도 있지만, 여러 타입의 뷰를 보여줘야 하는 경우도 존재한다. 예를 들면, 메시지 목록을 보여줄 때 텍스트, 이미지, 공지사항과 같이 타입에 따라 뷰도 다르게 생겨야 한다. 이를 ListView의 getView()로 구현할 수는 있지만, 코드가 매우 복잡해진다. 이러한 단점은 RecyclerView를 이용하여 작성하면 더욱 가독성이 좋은 코드를 작성할 수 있다.

```java
public class MyAdapter extends RecyclerView.Adapter<RecyclerView.ViewHolder> {

    private static final int VIEW_TYPE_TEXT = 0;
    private static final int VIEW_TYPE_IMAGE = 1;

    @Override
    public int getItemViewType(int position) {
        return position % 2 == 0 ? VIEW_TYPE_TEXT : VIEW_TYPE_IMAGE;
    }

    @Override
    public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        if (viewType == VIEW_TYPE_TEXT) {
            View itemView = LayoutInflater.from(parent.getContext())
                    .inflate(R.layout.list_item_text, parent, false);
            return new TextViewHolder(itemView);
        } else {
            View itemView = LayoutInflater.from(parent.getContext())
                    .inflate(R.layout.list_item_image, parent, false);
            return new ImageViewHolder(itemView);
        }
    }

    @Override
    public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {
        if (holder instanceof TextViewHolder) {
            ((TextViewHolder) holder).textView.setText("sample text");
        } else {
            ((ImageViewHolder) holder).imageView.setImageBitmap(/*sample bitmap*/);
        }
    }

    static class TextViewHolder extends RecyclerView.ViewHolder {
        private TextView textView;
        // generate constructor here
    }

    static class ImageViewHolder extends RecyclerView.ViewHolder {
        private ImageView imageView;
        // generate constructor here
    }
}
```

### 다양한 아이템 배치 방법

ListView에서는 아이템 목록을 수직 스크롤로 표시하는 방법밖에 없었다. 만약 수평 스크롤로 만들고 싶으면 CustomView를 구현해야만 했다. 하지만 RecyclerView에서는 LayoutManager를 이용해 아이템을 다양한 방법으로 배치할 수 있다.

```java
/* 수직 레이아웃 */
LinearLayoutManager verticalLayoutManager = new LinearLayoutManager(this);
recyclerView.setLayoutManager(verticalLayoutManager);

/* 수평 레이아웃 */
LinearLayoutManager horizontalLayoutManager = new LinearLayoutManager(this);
horizontalLayoutManager.setOrientation(LinearLayoutManager.HORIZONTAL);
recyclerView.setLayoutManager(horizontalLayoutManager);

/* 격자 레이아웃 */
GridLayoutManager gridLayoutManager = new GridLayoutManager(this, 3);
recyclerView.setLayoutManager(gridLayoutManager);
```


## OnClickListener

RecyclerView의 아이템을 클릭했을 때 무언가를 하고 싶으면 itemView에다 clickListener를 달아야 한다. 일반적으로 클릭했을 시에 아이템에 대한 정보를 보여주고 싶을 것이다. 리스너를 다는 방법(위치?)이 크게 두 가지가 있는데 어디가 좋을지 고민해본다.

### OnBindViewHolder()에 리스너를?

```java
@Override
public void onBindViewHolder(MyAdapter.ViewHolder holder, final int position) {
    // do something

    holder.itemView.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            Toast.makeText(v.getContext(), getItem(position), Toast.LENGTH_SHORT).show();
        }
    });
}
```

쉽게 떠오르는 위치는 onBindViewHolder 내부에서 itemView에 clickListener를 달아주는 것이다. 파라미터로 아이템의 position이 넘어오기 때문에 현재 아이템을 쉽게 찾을 수 있다. 하지만 이처럼 작성하면 final 값으로 바뀌고 아래와 같은 inspection 문구가 뜬다.

> Do not treat position as fixed; only use immediately and call holder.getAdapterPosition() to look it up later less... 
RecyclerView will not call onBindViewHolder again when the position of the item changes in the data set unless the item itself is invalidated or the new position cannot be determined.  For this reason, you should only use the position parameter while acquiring the related data item inside this method, and should not keep a copy of it.  If you need the position of an item later on (e.g. in a click listener), use getAdapterPosition() which will have the updated adapter position.

다른 이유로는 onBindViewHolder()는 모든 아이템에 대해 호출되는데, clickListener는 매번 반복할 필요가 없는 옵션이기 때문이다. 클릭했을 때 해당 position 정보만 정확히 알고 있으면 아이템을 찾을 수 있다.

### OnCreateViewHolder()에 리스너를!

[Recycler.ViewHolder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html) reference를 살펴보면 getAdapterPosition() 이라는 메소드가 있다. 그리고 이는 ViewHolder가 나타내는 항목의 어댑터 위치를 리턴한다. 이를 이용하면 원하는 position의 아이템을 찾아서 처리할 수 있겠다!

```java
@Override
public MyAdapter.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
    View itemView = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.list_item, parent, false);
    ViewHolder holder = new ViewHolder(itemView);
    holder.itemView.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            Toast.makeText(v.getContext(), getItem(position), Toast.LENGTH_SHORT).show();
        }
    });
    return holder;
}
```

또 다른 방법으로는 아이템 클릭에 대한 interface를 정의하여 ViewHolder 내부에서 리스너를 달 수도 있다. 이는 여러 타입의 ViewHolder가 있고 클릭 이벤트가 다를 경우 onCreateViewHolder() 크기가 커지는 것을 방지할 수 있다. 또한, MVP 패턴을 이용할 경우에 ItemClickListener를 Adapter가 아닌 Presenter에서 처리하도록 할 수 있어 테스트가 용이해진다.

```java
public interface ItemClickListener {
    void onItemClick(int position);
}
```

```java
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder> implements ItemClickListener {
    // override other abstract methods here

    @Override
    public MyAdapter.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View itemView = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.list_item, parent, false);
        return new ViewHolder(itemView, this);
    }

    @Override
    public void onItemClick(int position) {
        Toast.makeText(context, getItem(position), Toast.LENGTH_SHORT).show();
    }

    static class ViewHolder extends RecyclerView.ViewHolder {
        ViewHolder(View itemView, final ItemClickListener itemClickListener) {
            itemView.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    itemClickListener.onItemClick(getAdapterPosition());
                }
            });
        }
    }
}
```


## ViewHolder는 왜 static 이어야 할까?

ViewHolder를 사용할 때 non-static 으로 사용해도 문제없이 동작한다. 하지만 Android Developers 예제를 확인하면 static 클래스로 정의한다. 이는 안드로이드 개념보다 자바 개념을 알고 있어야 한다.

### Nested Class의 특징

위와 같이 Adapter 클래스 내부에 ViewHolder 클래스를 정의하는 것을 nested class라고 한다. Nested class에서 static을 붙이지 않으면 상위 클래스의 메소드나 변수를 사용할 수 있게 된다. 반면에 static nested class는 한 곳에서만 사용하는 클래스를 논리적으로 묶어서 처리하기 위해 사용한다. 즉, static을 붙임으로써 상위 클래스의 메소드나 변수를 사용하지 않겠다고 명시적으로 표시하는 것이다.

다른 이유로는 인스턴스를 클래스에서 재거할 때 생길 수 있는 메모리 릭을 피하기 위해서 정적 클래스로 사용한다. 앞서 말했듯이 non-static 클래스는 상위 클래스의 reference를 포함하고 있다. 즉, ViewHolder에 Adapter의 reference가 유지되기 때문에, 만약 Adapter가 종료되더라도 Garbage Collect 될 수 없게 되어 메모리 릭이 발생할 수도 있다. (이 부분은 조금 더 학습이 필요할 듯...)

### private static class?

만약 ViewHolder의 접근 지정자를 private로 선언하면 어떻게 될까? 직접 작성해보면 onCreateViewHolder() 내부에서는 문제없이 접근할 수 있다. 하지만, RecyclerView.Aapter를 상속받고 ViewHolder 클래스를 제네릭을 받는 부분에서 오류가 난다.

> ViewHolder has private access in MyAdapter.

```java
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder> {
    // override other abstract methods here

    private static class ViewHolder extends RecyclerView.ViewHolder {
        // generate constructor here
    }
}
```

접근 지정자의 개념은 파악하고 있으며 private로 정의되면 선언된 클래스 내부에서만 접근이 가능하다는 것도 알고 있다. 근데 이 부분에 대해 질문을 받았을 때, 클래스 선언부가 클래스 내부인가 아닌가에 대해 혼동이 왔다.

며칠동안 생각해보고 낸 결론은 클래스 선언부는 private이 접근 불가능한 영역이라는 것이다. onCreateViewHolder()는 MyAdapter 클래스 내부이기 때문에 접근이 가능했던 것이고 extends RecyclerView.Adapter< MyAdapter.ViewHolder > 는 MyAdater 클래스 선언부이기 때문에 불가능하다고 생각된다.


ps. 혹시 잘못된 내용이 있을 경우 알려주시면 감사하겠습니다.
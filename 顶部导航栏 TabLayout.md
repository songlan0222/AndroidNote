顶部导航栏 TabLayout

### 1、简述

1. 用处：主要用作顶部导航栏

2. 引入依赖

   > implemention 'com.android.support:design:25.3.1'`

3. 

### 2、引用组件

```xml
<android.support.design.widget.TabLayout
	android:id="@+id/tab_layout"
    android:layout_width="match_parent"
	android:layout_height="warp_content"/>
```

### 3、添加导航选项

#### ① 在 xml 中添加

> ```xml
> <android.support.design.widget.TabLayout
> 	android:id="@+id/tab_layout"
>     android:layout_width="match_parent"
> 	android:layout_height="warp_content">
>     
>     <android.support.design.widget.TabItem
> 		android:layout_width="wrap_content"
> 		android:layout_height="wrap_content"
> 		android:text="Tab1"/>
>     
> </android.support.design.widget.TabLayout>
> ```
>
> 说明：
>
> - text属性用于显示导航栏选项的名称；
> - **如果和ViewPager结合使用，发挥作用的将不再是text属性**

#### ② 在 kotlin 中引用

> ```kotlin
> tab_layout.addTab(tabLayout.newTab().setText("Tab1"))
> tab_layout.addTab(tabLayout.newTab().setText("Tab2"))
> ```



### 4、与ViewPager组件联动

**步骤一：引用组件**

1. **情形： 将TabLayout放置在ViewPager上方，此时会显示阴影效果**

   > ```xml
   > <android.support.design.widget.AppBarLayout
   > 	android:id="@+id/appbar"
   > 	android:layout_width="match_parent"
   > 	android:layout_height="wrap_content"
   > 	android:paddingTop="@dimen/appbar_padding_top"
   > 	android:theme="@style/AppTheme.AppBarOverlay">
   > 
   > 	……
   > 	
   > 	<android.support.design.widget.TabLayout
   > 		android:id="@+id/tabLayout"
   > 		android:layout_width="match_parent"
   > 		android:layout_height="wrap_content"
   > 		app:tabIndicatorColor="@color/colorAccent"
   > 		app:tabIndicatorHeight="2dp"
   > 		app:tabBackground="@android:color/white"
   > 		app:tabTextAppearance="@style/MyTabTextAppearance"
   > 		app:tabTextColor="@android:color/black"
   > 		app:tabSelectedTextColor="@android:color/holo_blue_light">
   > 	</android.support.design.widget.TabLayout>
   > 
   > </android.support.design.widget.AppBarLayout>
   > 
   > <android.support.v4.view.ViewPager
   > 	android:id="@+id/container"
   > 	android:layout_width="match_parent"
   > 	android:layout_height="match_parent"
   > 	app:layout_behavior="@string/appbar_scrolling_view_behavior"/>
   > ```
   >
   > 

2. **情形：将TabLayout放置在ViewPager内部，此时不会显示阴影效果**

   > ```xml
   > <androidx.viewpager.widget.ViewPager
   > 	android:id="@+id/viewPager"
   > 	android:layout_width="match_parent"
   > 	android:layout_height="match_parent"
   > 	android:layout_alignParentTop="true">
   > 
   > 	<com.google.android.material.tabs.TabLayout
   > 		android:id="@+id/tabLayout"
   > 		android:layout_width="wrap_content"
   > 		android:layout_height="45dp"
   > 		android:layout_gravity="top"
   > 		android:background="@android:color/white"
   > 		android:textColor="@color/black"/>
   > </androidx.viewpager.widget.ViewPager>
   > ```

**步骤二：创建适配器**

```kotlin
class ViewPagerAdapter(
    private val context: Context,
    private val titles: List<String>,
    private val data: List<String>
) : PagerAdapter() {
    
    override fun getCount() = data.size

    override fun isViewFromObject(view: View, `object`: Any): Boolean {
        return view == `object`
    }

    override fun instantiateItem(container: ViewGroup, position: Int): View {
        return when(position){
            0 -> {}
            1 -> {}
            2 -> {}
        }
        
    }

    override fun getPageTitle(position: Int): CharSequence? {
        // return super.getPageTitle(position)
        return titles[position]
    }

    override fun destroyItem(container: ViewGroup, position: Int, `object`: Any) {
        // super.destroyItem(container, position, `object`)
        container.removeView(`object` as View)
    }
}
```

说明：

- `instantiateItem()` 方法中，可以根据所在的标签不同，配置不同的页面

**步骤三：创建Fragment**

```kotlin
class PagerFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        val content: String = arguments?.getString("content") ?: ""
        val view = inflater.inflate(R.layout.item_text, container, false);
        val textView: TextView = view.findViewById(R.id.item_textView)
        textView.text = content
        Log.v("MyTest", "content= $content")
        return view
    }
}
```

**步骤四：绑定TabLayout和ViewPager**

```kotlin
// 绑定，要在viewPager设置完数据后，调用此方法，否则不显示 tabs文本
tabLayout.setupWithViewPager(viewPager);
```


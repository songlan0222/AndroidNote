视图翻页工具 ViewPager 

[toc]

# 1、概述

- 简介：视图翻页工具，提供多页面切换效果

- 导包：

- 使用：通过创建adapter填充多个view，在左右滑动时可以切换到不同的view

- 版本：

  - ViewPager

  - ViewPager 2,

    > 发布与2019年11月20日

- 依赖：
	- `implementation 'androidx.appcompat:appcompat:1.1.0'`

# 2、引用组件

```xml
<androidx.viewpager.widget.ViewPager
	android:id="@+id/mainViewPager"
	android:layout_width="match_parent"
	android:layout_height="match_parent"
</androidx.viewpager.widget.ViewPager>
```

说明：

- 一般情况下，ViewPager组件在界面上不需要配置其他参数，具体设置应当在后端代码中实现。

# 3、适配器

```kotlin
class TabFragmentAdapter(fm: FragmentManager): 
		FragmentStatePagerAdapter(fm, BEHAVIOR_RESUME_ONLY_CURRENT_FRAGMENT){
	// 必须重写
	override fun getCount(){
        ……
    }
	// 必须重写
	override fun getItem(position: Int): Fragment{
        return when(position){
            
        }
    }
    // 将ViewPager需要展示的界面都实例化
	override fun instanceItem(container: ViewGroup, position: Int): Any{
        val fragment = super.instanceItem(container, position) as Fragment
        return fragment
    }
    // ViewPager刷新fragment时，判断getItemPosition的状态     
    // - POSITION_UNCHANGED， 不会刷新
	// - POSITION_NONE，会刷新
	override fun getItemPosition(`object`: Any): Any{
        return POSITION_NONE
    }
}
```

# 4、配置监听器和适配器

```kotlin
viewPager.adapter = TabFragmentAdapter(supportFragmentManager)
viewPager.setOnclickListener(this)
……

override fun onPageSelected(position: Int){
    when (position) {
		0, 1, 3 -> mainBNV.menu.getItem(position).isChecked = true
		2 -> mainBNV.menu.getItem(position).isChecked = AppConfig.isShowRss
    }
}
```

# 5、联动BottomNavigationView配置Fragment

```kotlin
override fun onNavigationItemSelected(item: MenuItem): Boolean{
    when(item.itemId){
        R.id.bookshelf -> viewPager.setCurrentItem(0, false)
        ……
    }
}
```

说明：

- `setCurrentItem(0, false)`中
  - 0，表示获取`ViewPager`的`Adapter`中配置的`0号Fragment`；
  - false，表示关闭滑动效果；
  - 该方法最终会跳转到Adapter中，通过`getItem()方法`访问具体Fragment
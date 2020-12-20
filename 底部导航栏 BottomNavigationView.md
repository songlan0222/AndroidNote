底部导航栏 BottomNavigationView

## 依赖
- `implementation 'com.android.support:design:28.0.0'`
- ``
## 1、基本属性

- **itemBackgroud**
  指定的是底部导航栏的背景颜色,默认是主题的颜色。
- **itemIconTint**
  指的是导航栏中图片的颜色
- **itemTextColor**
  指的是导航栏文字的颜色
- **menu**
  导航栏内容布局

## 2、引用组件

```xml
<android.support.design.widget.BottomNavigationView
	android:id="@+id/bottom_navigation"
	android:layout_width="match_parent"
    android:layout_height="wrap_content" 
                                                    
    app:itemBackground="@color/colorAccent"
    app:itemIconTint="@color/status_text"
    app:itemTextColor="@color/yellow"
    app:menu="@menu/bottom_navigation_main">
</android.support.design.widget.BottomNavigationView>
```

说明：

- 如果`menu`中的`item-icon属性`采用的是<select>，此处可以不使用`itemIconTint属性`和`itemTextColor属性`

## 3、组件布局

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:app="http://schemas.android.com/apk/res-auto">
    <item
        android:id="@+id/action_favorites"
        android:enabled="true"
        android:icon="@mipmap/canvas"
        android:title="@string/text_favorites"
        app:showAsAction="ifRoom" />
    <item
        android:id="@+id/action_schedules"
        android:enabled="true"
        android:icon="@mipmap/ic_launcher"
        android:title="@string/text_schedules"
        app:showAsAction="ifRoom" />
    <item
        android:id="@+id/action_music"
        android:enabled="true"
        android:icon="@mipmap/ic_launcher"
        android:title="@string/text_music"
        app:showAsAction="ifRoom" />
</menu>
```

## 4、为导航栏选项配置点击变化

```xml
<selector xmlns:android="http://schemas.android.com/apk/res/android">
	<item android:drawable="@drawable/ic_bottom_books_e" android:state_checked="false"/>
    <item android:drawable="@drawable/ic_bottom_books_s" andorid:state_checked="true"/>
</selector>
```

说明：

- drawable属性，可以配置为png，也可以配置为`Vector Assets`矢量集

## 5、点击事件

### ① 单次点击事件

```kotlin
override onNavigationItemSelected(item: MenuItem): Boolean{
	when(item.itemId){
		R.id.main_bookshelf -> mainViewPager.setCurrentItem(0, false)
        ……
	}
    return true
}
```

说明：

- 此处是联动ViewPager，通过点击bnv的导航选项为ViewPager动态配置Fragment
- 此处的返回值可以true / false，区别在于：
  - 如果返回值为true，则点击时会调用默认的`ViewPager`的`onPageSelected事件`
  - 如果返回值为false，则需要`override onPageSelected事件`

### ② 再次点击事件

```kotlin
override onNavigationItemReselected(item: MenuItem){
    when(item.itemId){
        ……
    }
}
```


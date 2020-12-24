CheckBox 自定义样式

应该使用`android:button` ，而不是`android:background`

**自定义Style**

```xml
<style name="CustomCheckBoxTheme" 
       parent="@android:style/Widget.CompoundButton.CheckBox">
   <item name="android:button">@drawable/checkbox_style</item> 
</style>
```

**添加Style**

```xml
<CheckBox
	android:id="@+id/tv_upper"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	android:gravity="center_vertical"
	style="@style/CustomCheckBoxTheme" />
```


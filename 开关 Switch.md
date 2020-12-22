1. 组件引用

   ```xml
   <Switch
       android:layout_width="wrap_content"
       android:layout_height="wrap_content" />
   ```

2. 自定义样式

   ```xml
   <Switch
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:thumb="@drawable/switch_custom_thumb_selector"
       android:track="@drawable/switch_custom_track_selector" />
   ```

   说明：

   - thumb，表示中间开关属性
   - track，表示滑动轨道属性

3. 切换选中状态

   ```kotlin
   signNotifySwitch.isChecked = true
   signNotifySwitch.isChecked = false
   ```

4. 选中事件

   ```kotlin
   signNotifySwitch.setOnCheckedChangeListener { buttonView, isChecked ->
       saveToProfile(AppProfiles.SIGN_NOTIFY, isChecked)
   }
   ```

5. 显示文字

6. ```kotlin
   <Switch
       ……
       android:showText="true"
       android:switchTextAppearance="@style/SwitchTheme"
       android:textOff="OFF"
       android:textOn="ON"
   	android:switchTextAppearance="@style/SwitchTheme"
       ……
   />
   ```

   说明：

   - `android:showText="true"`，开启文字显示功能

   - `android:textOn`/`android:textOff`，分别设置开启和关闭状态的文字

   - `android:switchTextAppearance="@style/SwitchTheme"`，配置文字颜色

     - 在res文件夹下建一个color文件夹

     - color文件夹中定义文本颜色状态文本：switch_text_selector.xml
     
       ```xml
       <?xml version="1.0" encoding="utf-8"?>
       <selector xmlns:android="http://schemas.android.com/apk/res/android">
           <item android:color="#FFF" android:state_checked="false" />
           <item android:color="#000" android:state_checked="true" />
  </selector>
       ```

     - 在style文件中定义样式
     
       ```xml
  <style name="SwitchTheme" 
              parent="@android:style/TextAppearance.Small">
           <item name="android:textColor">
               @color/switch_text_selector
           </item>
       </style>
       ```
       
       
     
     
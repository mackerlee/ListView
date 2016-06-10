# ListView

android 68 lesson
1.ListView组件：Android组件中最常用的一种视图组件，是以垂直列表的方式列出需要显示的列表项.该组件常用属性有：
  --android:cacheColorHint="#00000000"      //设置拖动背景色为透明
  --android:dividerHeight="30px"            //listView item之间的高度
  --android:divider="@drawable/ic_launcher" //listView item之间的背景或者说颜色
  --android:fadingEdge="vertical"           //上边和下边有黑色的阴影，值为none的话就没有阴影
  --android:drawSelectorOnTop="false"       //点击某条记录不放，颜色会在记录的后面，成为背景色
  --android:scrollbars="horizontal|none"    //只有值为horizontal|vertical的时候才会显示滚动条，并且会自动隐藏和显示
  --android:fastScrollEnable="true"         //快速滚动效果，配置类该属性在快速滚动式会在旁边出现一个小方块快速滚动效果
  --android:listSelector="@color/pink"      //listView item选中时的颜色
  --android:entries="@array/citys"          //设置列表填充的内容
  
2.ListView实例：在res->layout->activity_main.xml中设置：
  <?xml version="1.0" encoding="utf-8"?>
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:tools="http://schemas.android.com/tools"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:paddingBottom="@dimen/activity_vertical_margin"
      android:paddingLeft="@dimen/activity_horizontal_margin"
      android:paddingRight="@dimen/activity_horizontal_margin"
      android:paddingTop="@dimen/activity_vertical_margin"
      tools:context="com.example.mackerlee.android_68.MainActivity">
  
      //--设置一个试图列表组件
      <ListView
          android:id="@+id/listview01"
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          //--填充列表的内容，引用数组,在res->values->strings.xml中定义的名称为names的数组
          android:entries="@array/names"
          //--设置两个列表内容之间的填充颜色或图片
          android:divider="#ffffbb"
          //--设置两个列表内容之间的间距
          android:dividerHeight="20dp"
          android:drawSelectorOnTop="true">
      </ListView>
    </RelativeLayout>
    
    在res->values->strings.xml中设置需要用于填充列表的名为names的数组
    <resources>
      <string name="app_name">Android_68</string>
      
      //--设置数组
      <string-array name="names">
          <item>刘德华</item>
          <item>张学友</item>
          <item>李连杰</item>
          <item>张国荣</item>
          <item>成龙</item>
          <item>山口百惠</item>
      </string-array>
  </resources>

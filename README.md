# ListView

android 68-69 lesson
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
          //--设置选中项的时候不会被下面设置的背景色覆盖掉
          android:drawSelectorOnTop="false"
          //--设置选中项时的背景颜色为蓝色
          android:listSelector="#00ffff">
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
  
  在app->java->包名->MainActivity.java中设置单击响应事件:
  package com.example.mackerlee.android_68;
  
  import android.support.v7.app.AppCompatActivity;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.AdapterView;
  import android.widget.ListView;
  import android.widget.TextView;
  import android.widget.Toast;
  
  //--设置ListView的选项单击响应事件接口implements AdapterView.OnItemClickListener
  public class MainActivity extends AppCompatActivity implements AdapterView.OnItemClickListener{
      private ListView lv;
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          lv = (ListView)findViewById(R.id.listview01);
          //--注册ListView的响应事件
          lv.setOnItemClickListener(this);
      }
  
      //--实现ListView中点击事件的响应事件方法
      //----参数：AdapterView<?> parent就是指ListView， View view就是单击的选项的视图
      //---------position就是单击Item项的位置第几个Item项，id就是选择Item项的编号
      @Override
      public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
          System.out.println("parent = "+parent.getClass());
          //--打印出选中项的类型，其实就是TextView
          System.out.println("View = "+view.getClass());
          System.out.println("position = "+position);
          System.out.println("id = "+id);
          //--获取选中项的值
          TextView tv = (TextView)view;
          Toast.makeText(this,tv.getText(),Toast.LENGTH_LONG).show();
      }
  }

android 70 lesson
1.ListActivity:上面在xml中设置的ListView还可以放置其他组件，但实际运用中我们常用ListView就一个组件而不会用其他的组件，因此
               android中提供类一个ListActivity类，就是一个只有ListView组件的界面。它的默认布局是由一个单一的在屏幕中心的全屏幕的列表，用setcontentview()在oncreate()设置您自己的自定义屏幕布局视图布局，必须包含一个列表视图的对象
               ID:"@android:id/list"(这是固定的不能是别的)，自定义视图对象列表视图是空的，必须包含一个视图对象的
               ID：android:id/empty
2.ListActivity实例：在jiava->包名->新建MainActivity2.java：
  package com.example.mackerlee.android_68;

  import android.app.ListActivity;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.AdapterView;
  import android.widget.ArrayAdapter;
  import android.widget.ListView;
  
  //--ListActivity的ID是必须是list
  public class MainActivity2 extends ListActivity{
  
      //--不需要写布局，因为ListActivity已经提供了一个默认的包含ListView组件的布局
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          //--ListActivity中已经包含一个默认布局，所以不要用setContentView(R.layout.activity_main);
          String[] names = {"李连杰","成龙","张国荣","希斯莱杰"};
          //--填充数据依然采用适配器来实现
          ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,android.R.layout.simple_list_item_1,names);
          //--将适配器绑定到ListView组件上
          setListAdapter(adapter);
      }
  
      //--在ListActivity类中已经实现了OnItemClickListener，所以只要重写ListActivity监听方法即可，而不要再重新继承接口：
      @Override 
      protected void onListItemClick(ListView l, View v, int position, long id) {
          super.onListItemClick(l, v, position, id);
      }
  }
  在manifests->AndroidManifest.xml中更改启动的Activity:
  <?xml version="1.0" encoding="utf-8"?>
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.example.mackerlee.android_68">
  
      <application
          android:allowBackup="true"
          android:icon="@mipmap/ic_launcher"
          android:label="@string/app_name"
          android:supportsRtl="true"
          android:theme="@style/AppTheme">
          <activity android:name=".MainActivity2"> //将默认的更改为MainActivity2便于启动测试
              <intent-filter>
                  <action android:name="android.intent.action.MAIN" />
  
                  <category android:name="android.intent.category.LAUNCHER" />
              </intent-filter>
          </activity>
      </application>
  
  </manifest>

 

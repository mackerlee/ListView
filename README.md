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

android 71 lesson
1.单选模式：带单选按钮的，
2.实例实现单选模式：在res->layout->创建一个新的布局activity_main3.xml：
  <?xml version="1.0" encoding="utf-8"?>
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <ListView
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:id="@+id/listView02"
          android:layout_alignParentTop="true"
          android:layout_alignParentStart="true" />
  </RelativeLayout>
  在java->包名->创建一个新的MainActivity3.java：
  package com.example.mackerlee.android_68;

  import android.os.Bundle;
  import android.support.v7.app.AppCompatActivity;
  import android.widget.ArrayAdapter;
  import android.widget.ListView;
  
  /**
   * Created by mackerlee on 2016/6/14.
   */
  public class MainActivity3 extends AppCompatActivity {
      private ListView lv;
  
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main3);
          lv = (ListView)findViewById(R.id.listView02);
          String[] citys = {"北京","上海","广州"};
          //--创建单选布局适配器，内容填充上面的citys
          ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,android.R.layout.simple_list_item_single_choice,citys);
          //--设置ListView为单选模式
          lv.setChoiceMode(ListView.CHOICE_MODE_SINGLE);
          //--将该listview与适配器绑定
          lv.setAdapter(adapter);
      }
  }
  在配置清单列表中设置MainActivity3.java为启动Activity：
  <?xml version="1.0" encoding="utf-8"?>
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.example.mackerlee.android_68">
  
      <application
          android:allowBackup="true"
          android:icon="@mipmap/ic_launcher"
          android:label="@string/app_name"
          android:supportsRtl="true"
          android:theme="@style/AppTheme">
          <activity android:name=".MainActivity3"> //设置启动Activity
              <intent-filter>
                  <action android:name="android.intent.action.MAIN" /> 
                  <category android:name="android.intent.category.LAUNCHER" />
              </intent-filter>
          </activity>
      </application>
  
  </manifest>

3.多选列表实例：其余不变，在MainActivity3.java中将单选改为多选模式：
  package com.example.mackerlee.android_68;

  import android.os.Bundle;
  import android.support.v7.app.AppCompatActivity;
  import android.view.View;
  import android.widget.AdapterView;
  import android.widget.ArrayAdapter;
  import android.widget.ListView;
  
  /**
   * Created by mackerlee on 2016/6/14.
   */
  public class MainActivity3 extends AppCompatActivity implements AdapterView.OnItemClickListener{
      private ListView lv;
  
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main3);
          lv = (ListView)findViewById(R.id.listView02);
          String[] citys = {"北京","上海","广州","深圳"};
  
          /**
           *  单选ListView实现
          //--创建单选布局适配器，内容填充上面的citys
          ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,android.R.layout.simple_list_item_single_choice,citys);
          //--设置ListView为单选模式
          lv.setChoiceMode(ListView.CHOICE_MODE_SINGLE);
          //--将该listview与适配器绑定
          lv.setAdapter(adapter);
           */
          //--创建多选布局适配器，内容填充上面的citys
          ArrayAdapter<String> adapterMultiple = new ArrayAdapter<String>(this,android.R.layout.simple_list_item_multiple_choice,citys);
          //--设置ListView为多选模式
          lv.setChoiceMode(ListView.CHOICE_MODE_MULTIPLE);
          //--将该listview与适配器绑定
          lv.setAdapter(adapterMultiple);
          //--注册单击事件
          lv.setOnItemClickListener(this);
      }
  
      //--实现单击响应事件方法
      @Override
      public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
          System.out.println(view.getClass()); //可知是CheckedTextView，可用于判断是否被选中
      }
  }

android 72-73 ListView实现图文列表
1.使用SimpleAdapter建立复杂的列表项：
   SimpleAdapter(Context context,List<?extends Map<String,?>>data,int resource,String[]from,int[]to)
   参数：context:SimpleAdapter关联的View的运行环境;
         data：一个Map组成的List,在列表中的每个条目对应列表中的一行，每个一个map中应该包含所有在from参数中指定的键;
         resource:一个定义列表项的布局文件的资源ID，布局文件至少包含那些在to中定义了的ID;
         from:一个被添加到Map映射上的键名;
         to:将绑定数据的视图的ID，跟from参数对应，这些应该全是TextView;
2.每一个item就是一个map,多个map装在一个list中来组装一个列表项.
3.SimpleAdapter实例：在res->layout->创建一个新的ListView组件布局activity_main72.xml：
  <?xml version="1.0" encoding="utf-8"?>
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <ListView
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:id="@+id/listView72"
          android:layout_alignParentTop="true"
          android:layout_alignParentStart="true" />
  </RelativeLayout>
  创建一个新的Activity在main->java->包名->MainActivity72.java：
  package com.example.mackerlee.android_68;

  import android.os.Bundle;
  import android.support.v7.app.AppCompatActivity;
  import android.widget.ListView;
  import android.widget.SimpleAdapter;
  
  import java.util.ArrayList;
  import java.util.HashMap;
  import java.util.List;
  import java.util.Map;
  
  /**
   * Created by mackerlee on 2016/6/15.
   */
  public class MainActivity72 extends AppCompatActivity {
  
      private ListView lv;
  
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main72);
          lv = (ListView)findViewById(R.id.listView72);
  
          //--一个列表项的内容，即一个Iitem
          Map<String,Object> item1 = new HashMap<>();
          item1.put("image",R.drawable.jt1); //放入一个图片，image是标记，可以随便取
          item1.put("name","小白"); //以上三个组成一个列表项
  
          //--第二个列表项的内容，即一个Iitem
          Map<String,Object> item2 = new HashMap<>();
          item2.put("image",R.drawable.yx2); //放入一个图片
          item2.put("name","小黑"); //以上三个组成一个列表项
  
          //--构造List内容
          List<Map<String, Object>> data = new ArrayList<Map<String, Object>>();
          data.add(item1);
          data.add(item2);
  
          //--后面两个参数是指建立从new String[]{"image","name"}到new int[]{R.id.imageview01,R.id.textview01}的映射
          //----String[]{"image","name"}是item标签中的标签名称，
          //----int[]{R.id.imageview01,R.id.textview01}是item布局的id,表示image放到imageview01,name放到textview01
          SimpleAdapter simpleAdapter = new SimpleAdapter(this,data,R.layout.main72_item,
                  new String[]{"image","name"},new int[]{R.id.imageview01,R.id.textview01});
          //--SimpleAdapter用于适配ListView的自定义样式布局,绑定ListView
          lv.setAdapter(simpleAdapter);
      }
  }
  在res->layout->创建一个ListView中的每个item的布局main72_item.xml:
  <?xml version="1.0" encoding="utf-8"?>
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="wrap_content">
  
      <ImageView
          android:id="@+id/imageview01"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:src="@drawable/yx2" />
      <TextView
          android:id="@+id/textview01"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:text="文字说明"
          android:layout_toRightOf="@id/imageview01"/>
  
  </RelativeLayout>
  在mainfests->AAndroidMainifest.xml中更改启动的activity:
  <?xml version="1.0" encoding="utf-8"?>
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.example.mackerlee.android_68">
  
      <application
          android:allowBackup="true"
          android:icon="@mipmap/ic_launcher"
          android:label="@string/app_name"
          android:supportsRtl="true"
          android:theme="@style/AppTheme">
          <activity android:name=".MainActivity72"> //启动项设置为main->java->包名->MainActivity72.java
              <intent-filter>
                  <action android:name="android.intent.action.MAIN" />
  
                  <category android:name="android.intent.category.LAUNCHER" />
              </intent-filter>
          </activity>
      </application>

</manifest>

android 74 lesson
1.BaseAdapter实现更加灵活的列表：可以实现比simpleAdapter更复杂的列表布局，由于BaseAdapter是一个抽象类，使用该类需要自己
            写一个适配器继承该类,使得每一个步骤都可控可自行定义。SimpleAdapter其实就是继承类BaseAdapter。需要实现4个方法
    --public int getCount():获取列表项的总数;
    --public Object getItem(int arg0):根据位置获取每个列表项对象；
    --public long getItemId(int position):根据位置获取每个列表项的id;
    --public View getView(int position,View convertView,ViewGroup parent):创建并返回一个item视图
2.ListView的优化：
  --重复使用convertView；
  --使用ViewHolder提高在容器中查找组件的效率。
3.自定义组件实例:
  在res->layout->新创建一个ListView组件activity_main74.xml：
  <?xml version="1.0" encoding="utf-8"?>
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <ListView
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:id="@+id/listView74"
          android:layout_alignParentTop="true"
          android:layout_alignParentStart="true" />
  </RelativeLayout>
  
  在res->layout->新建一个item的视图组件main74_item.xml:
  <?xml version="1.0" encoding="utf-8"?>
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="wrap_content">
  
      <ImageView
          android:id="@+id/imageview74"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:src="@drawable/yx2" />
      <TextView
          android:id="@+id/textview74"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:text="文字说明"
          android:layout_toRightOf="@id/imageview74"/>
  
  </RelativeLayout>
  
  在java->包名->新建一个MainActivity74.java：
  package com.example.mackerlee.android_68;

  import android.os.Bundle;
  import android.support.v7.app.AppCompatActivity;
  import android.view.View;
  import android.view.ViewGroup;
  import android.widget.BaseAdapter;
  import android.widget.ImageView;
  import android.widget.ListView;
  import android.widget.TextView;
  
  /**
   * Created by mackerlee on 2016/6/19.
   */
  public class MainActivity74 extends AppCompatActivity {
  
      private ListView lv;
  
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main74);
          lv = (ListView)findViewById(R.id.listView74);
          lv.setAdapter(new MyAdapter());
      }
  
      //--
      private int[] images = {R.drawable.jt1,R.drawable.yx2,R.drawable.jt1,R.drawable.yx2,R.drawable.jt1,R.drawable.yx2,R.drawable.jt1,R.drawable.yx2};
      private String[] names = {"刘德华","张国荣","周星驰","成龙","林青霞","梁家辉","周润发","梁朝伟"};
  
      //--自定义适配器,实现其四个方法
      class MyAdapter extends BaseAdapter{
  
          //--获取列表项的总数
          @Override
          public int getCount() {
              return names.length;
          }
  
          //--根据位置获取每一个列表项对象
          @Override
          public Object getItem(int position) { //返回类型是Object，即可以是任意类型返回值
              return names[position];
          }
  
          //--根据位置获取每个列表项的id
          @Override
          public long getItemId(int position) {
              return position; //id和position数据是一样的但性质不同
          }
  
          //--通过getView创建并返回一个视图
          //----参数：position位置，converview转换视图，ViewGroup parent父组件，没有就空NULL
          //----getView方法会被循环调用，不断加载列表项用于显示,如果有滚动条不断滑动滚动条会不停创建刷新的列表项
          //----因此存在性能影响必须进行优化,两种优化方式：
          //--------1.重复使用convertView参数:类似扶手电梯的原理，将看不见的列表item给下一个拉出来的item用.
          //--------2.使用ViewHolder提高在容器中查找组件的效率
          @Override
          public View getView(int position, View convertView, ViewGroup parent) {
              //--convertView如果为null表示满屏显示的item的7个VIEW没有空闲的view，当上拉或下拉时
              //-----消失的那个就会给convertView，因此可以利用convertView，而无须再次重建.
              System.out.println("---------------------"+convertView);
              System.out.println("position="+position);
  
              //--实现这几个对象的来回利用重复赋值
              if(convertView == null){
                  //--获取布局填充器,getLayoutInflater通过调用inflate方法来找res/layout/下的xml布局文件，并且实例化
                  //----inflate方法：用于动态加载布局，其参数是View的layout的ID和root生成的层次结构的根视图
                  //-----------------本例中没有父视图了所以用null
                  //View view = getLayoutInflater().inflate(R.layout.main74_item,null);
                  convertView = getLayoutInflater().inflate(R.layout.main74_item,null);
                  ViewHolder vh = new ViewHolder();
                  vh.iv = (ImageView)convertView.findViewById(R.id.imageview74);
                  vh.tv = (TextView)convertView.findViewById(R.id.textview74);
                  //--设置convertView的标记,这样就不需要每次都findViewById了，提高了性能
                  convertView.setTag(vh);
              }else{
                  //--从标记中获取vh
                  ViewHolder vh = (ViewHolder)convertView.getTag();
                  vh.iv.setImageResource(images[position]);
                  vh.tv.setText(names[position]);
              }
              //System.out.println(convertView); //看看每次创建的对象是否一样
              //--如果按下面这种写法，findViewById每次都要去converView中重新查找
              //ImageView iv = (ImageView)convertView.findViewById(R.id.imageview74);  //注意只有设置主布局时可以直接用findViewById,其他都要用对象调用findViewById
              //TextView tv = (TextView)convertView.findViewById(R.id.textview74);
  
              //--填充iv和tv资源
              //iv.setImageResource(images[position]);
              //tv.setText(names[position]);
  
              return convertView;
          }
      }
  
      //--定义一个内部类，在该类中定义视图中所要拥有的组件
      static class ViewHolder{
          ImageView iv;
          TextView tv;
      }
  }

  在AndroidManifest.xml中修改启动项：
  <?xml version="1.0" encoding="utf-8"?>
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.example.mackerlee.android_68">
  
      <application
          android:allowBackup="true"
          android:icon="@mipmap/ic_launcher"
          android:label="@string/app_name"
          android:supportsRtl="true"
          android:theme="@style/AppTheme">
          <activity android:name=".MainActivity74">
              <intent-filter>
                  <action android:name="android.intent.action.MAIN" />
  
                  <category android:name="android.intent.category.LAUNCHER" />
              </intent-filter>
          </activity>
      </application>
  
  </manifest>

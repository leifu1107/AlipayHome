# AlipayHome
高仿支付宝首页的头部伸缩动画(使用design实现效果,CoordinatorLayout+AppBarLayout+CollapsingToolbarLayout+Toolbar)

### 效果图(效果图渐变不明显,建议下载demo查看)
![](https://github.com/leifu1107/AlipayHome/raw/master/screenshots/1.gif) 
### 实现原理(一个CoordinatorLayout嵌套AppBarLayout再嵌套CollapsingToolbarLayout再嵌套Toolbar的布局)
1、CoordinatorLayout嵌套AppBarLayout，这是为了让头部导航栏能够跟随内容视图下拉而展开，跟随内容视图上拉而收缩。这个内容视图可以是RecyclerView，也可以是NestedScrollView；<br>
2、AppBarLayout嵌套CollapsingToolbarLayout，这是为了定义导航栏下面需要展开和收缩的部分视图；<br>
3、CollapsingToolbarLayout嵌套Toolbar，这是为了定义导航栏上方显示的搜索区域或小图标区域，其中Toolbar还要定义两个不同的样式布局，用于分别显示搜索区域或小图标区域。<br>

### 布局文件如下

```java
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context="leifu.alipayhome.MainActivity">

    <android.support.design.widget.AppBarLayout
        android:id="@+id/abl_bar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <android.support.design.widget.CollapsingToolbarLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_scrollFlags="scroll|exitUntilCollapsed|snap">

            <include
                layout="@layout/include_title_big"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="50dp"
                app:layout_collapseMode="parallax"
                app:layout_collapseParallaxMultiplier="0.7"/>

            <android.support.v7.widget.Toolbar
                android:layout_width="match_parent"
                android:layout_height="50dp"
                app:contentInsetLeft="0dp"
                app:contentInsetStart="0dp"
                app:layout_collapseMode="pin">

                <include
                    android:id="@+id/include_toolbar_search"
                    layout="@layout/include_toolbar_search"/>

                <include
                    android:id="@+id/include_toolbar_small"
                    layout="@layout/include_toolbar_small"/>
            </android.support.v7.widget.Toolbar>
        </android.support.design.widget.CollapsingToolbarLayout>
    </android.support.design.widget.AppBarLayout>

    <android.support.v4.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical">

            <ImageView
                android:layout_width="match_parent"
                android:layout_height="220dp"
                android:scaleType="centerCrop"
                android:src="@drawable/menu"/>

            <ImageView
                android:layout_width="match_parent"
                android:layout_height="580dp"
                android:scaleType="centerInside"
                android:src="@drawable/bg"/>

            <TextView
                android:layout_width="match_parent"
                android:layout_height="350dp"
                android:gravity="center_horizontal"
                android:padding="20dp"
                android:text="上下可以滑动"/>
        </LinearLayout>
    </android.support.v4.widget.NestedScrollView>

</android.support.design.widget.CoordinatorLayout>
```
* ## `注意`
1.使用之前要在 /app/build.gradle中添加
 ```java
  	dependencies {
	        compile 'com.android.support:design:26.0.0-alpha1'
            compile 'com.gyf.barlibrary:barlibrary:2.3.0'
	}
```
design为材料设计仓库,barlibrary为第三方沉浸式状态栏的实现<br>

2.CollapsingToolbarLayout中需要添加
```java
		app:layout_scrollFlags="scroll|exitUntilCollapsed|snap"
```

3.include_title_big(扫一扫 付一付 聊一聊 咻一咻 的大图标)的布局需要 
```java
		android:layout_marginTop="50dp"
                app:layout_collapseMode="parallax"
                app:layout_collapseParallaxMultiplier="0.7" 
```
layout_marginTop预留出toolbar的高度,否则布局重叠<br>

4.Toolbar
```java
		android:layout_height="50dp"
                app:contentInsetLeft="0dp"
                app:contentInsetStart="0dp"
                app:layout_collapseMode="pin"
```
高度为50dp,app:layout_collapseMode="pin"为固定模式<br>
Toolbar里面放两个布局(一个搜索的布局,一个扫一扫 付一付 聊一聊 咻一咻 的小图标布局),通过监听AppBarLayout的移动控制显示还是隐藏<br>
5.最下面防止滚动的控件,可以是RecyclerView，也可以是NestedScrollView,一定要在布局中设置
```java
app:layout_behavior="@string/appbar_scrolling_view_behavior"
```
	

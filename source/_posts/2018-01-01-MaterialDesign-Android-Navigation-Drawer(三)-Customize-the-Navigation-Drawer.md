---
title: '[Material Design] Android Navigation Drawer(三) - Customize the Navigation Drawer'
date: 2018-01-01 14:56:36
tags: ['Android','Material Design']
---

## Introduction

在本篇文章中，我們要客製化實現數種 Navigation Drawer 展開時的顯示方式<!-- more -->

### Placing the Navigation Drawer inside status bar

接下來我們要設定 Navigation Drawer 展開時會符合整個螢幕，不會受到 status bar 限制

<img src="navigationDrawerCustomize01.png" width=50% height=50% align=center/>

首先我們打開 values 目錄下的 styles.xml 文件，創建一個新 style name = "AppTheme.NoActionBar"
<img src="navigationDrawerCustomize02.png" width=100% height=100% align=center/>

接下來我們打開 manifests 目錄下 AndroidManifest.xml 文件，修改我們顯示 Navigation drawer 的 Activity Theme 為剛剛創建的 style
<img src="navigationDrawerCustomize03.png" width=100% height=100% align=center/>

接著回到剛剛創建的 style tag 下，增加幾條屬性
``` bash
<resources>

	...

    <style name="AppTheme.NoActionBar">
        <!--此屬性表示 Activity 不會有預設的 Toolbar-->
        <item name="windowActionBar">false</item>
        <!--此屬性表示 Activity 預設的 Toolbar 將不會有 label-->
        <item name="windowNoTitle">true</item>
        <item name="android:windowDrawsSystemBarBackgrounds">true</item>
        <!--此屬性表示 status bar 背景為透明-->
        <item name="android:statusBarColor">@android:color/transparent</item>
    </style>

</resources>

```

運行效果如下: 
<img src="navigationDrawerCustomize04.png" width=50% height=50% align=center/>

### Placing the Navigation Drawer under the Toolbar

接下來我們要設定 Navigation Drawer 展開時，不會覆蓋到 Toolbar，會在其下方

首先，我們要先把上一部分新增的 style 移除，並將 manifests 的 Activity theme屬性移除
<img src="navigationDrawerCustomize05.png" width=100% height=100% align=center/>
<img src="navigationDrawerCustomize06.png" width=100% height=100% align=center/>

緊接著我們打開 layout目錄下 navigation_drawer.xml 文件，並在最外層新增一個 view group 佈局
<img src="navigationDrawerCustomize07.png" width=100% height=100% align=center/>

接下來我們把 import activity 的 include tag 搬到最外層的 view group tag 下，並把 DrawerLayout tag 中修改屬性 android:fitsSystemWindows 為 false 與新增屬性 android:layout_marginTop="?attr/actionBarSize"
<img src="navigationDrawerCustomize08.png" width=100% height=100% align=center/>

``` bash
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    ...
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <include layout="@layout/activity_main"/>

    <android.support.v4.widget.DrawerLayout
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/drawer_layout"
        android:layout_marginTop="?attr/actionBarSize"
        android:fitsSystemWindows="false"
        tools:openDrawer="start">

        <android.support.design.widget.NavigationView
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:id="@+id/navigation_view"
            android:layout_gravity="start"
            android:fitsSystemWindows="true"
            app:headerLayout="@layout/navigation_header"
            app:menu="@menu/navigation_menu"/>

    </android.support.v4.widget.DrawerLayout>

</RelativeLayout>
```

運行效果如下:
<img src="navigationDrawerCustomize09.gif" width=50% height=50% align=center/>

這時候我們想要在 Navigation Drawer 打開時，讓背景 activity 顯示為灰色，首先需要把 include activity tag 重新放回 DrawerLayout tag 中，並移除屬性  android:layout_marginTop="?attr/actionBarSize"，最後一步在 NavigationView tag 下新增屬性 android:layout_marginTop="?attr/actionBarSize" 與修改屬性 android:fitsSystemWindows 為 false

<img src="navigationDrawerCustomize10.png" width=100% height=100% align=center/>

``` bash
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout 
    ...
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <android.support.v4.widget.DrawerLayout
        android:id="@+id/drawer_layout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:fitsSystemWindows="false"
        android:orientation="vertical"
        tools:openDrawer="start">

        <include layout="@layout/activity_main" />

        <android.support.design.widget.NavigationView
            android:id="@+id/navigation_view"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_gravity="start"
            android:layout_marginTop="?attr/actionBarSize"
            android:fitsSystemWindows="false"
            app:headerLayout="@layout/navigation_header"
            app:menu="@menu/navigation_menu" />

    </android.support.v4.widget.DrawerLayout>

</RelativeLayout>
```

運行效果如下:
<img src="navigationDrawerCustomize11.gif" width=50% height=50% align=center/>

可能你又有以下需求，我只要 activity 背景變暗就好，Toolbar 不用

首先在 res -> layout 目錄下新增一個 toolbar_layout.xml
<img src="navigationDrawerCustomize12.png" width=100% height=100% align=center/>

接著把 activity 中的 toolbar tag 移除
<img src="navigationDrawerCustomize13.png" width=100% height=100% align=center/>

再回到 navigation_drawer.xml 文件下，將剛剛新增的 toolbar_layout.xml 利用 include tag import 進來，並將 NavigationView tag 的屬性 android:layout_marginTop="?attr/actionBarSize" 移動到 DrawerLayout tag 中
<img src="navigationDrawerCustomize14.png" width=100% height=100% align=center/>

``` bash
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    ...
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <include layout="@layout/toolbar_layout"/>

    <android.support.v4.widget.DrawerLayout
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/drawer_layout"
        android:layout_marginTop="?attr/actionBarSize"
        android:fitsSystemWindows="false"
        tools:openDrawer="start">

        <include layout="@layout/activity_main"/>

        <android.support.design.widget.NavigationView
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:id="@+id/navigation_view"
            android:layout_gravity="start"
            android:fitsSystemWindows="false"
            app:headerLayout="@layout/navigation_header"
            app:menu="@menu/navigation_menu"/>

    </android.support.v4.widget.DrawerLayout>

</RelativeLayout>
```

運行效果如下:
<img src="navigationDrawerCustomize15.gif" width=50% height=50% align=center/>

參考網站:
[Android Developer](https://developer.android.com/training/implementing-navigation/nav-drawer.html)
[Material Design](https://material.io/guidelines/patterns/navigation-drawer.html)
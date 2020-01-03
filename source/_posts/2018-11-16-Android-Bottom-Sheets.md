---
title: '[Android] Bottom Sheets'
date: 2018-11-16 17:06:46
tags: ['Android']
---
## Introduction
`BottomSheets`是`Design Support Library23.2`版本引入的一個底部彈出框控件，通常用於顯示主畫面之外的額外訊息<!-- more -->，默認是隱藏，或只顯示一小部分，並可以透過代碼或是手勢，來控制視圖是否要展開，本文主要是介紹如何使用`BottomSheets`，更多介紹可以查看[`Material-Components`](https://material.io/design/components/sheets-bottom.html#anatomy)
[`Material-Design`](https://material.io/design/components/sheets-bottom.html#)

## The type of bottom sheets
`BottomSheets`可以分為以下兩種類型。
### Persistent Bottom Sheet
為主畫面的一份部內容，默認被隱藏，只會顯示出一部分的內容，其高度`elevation`與主畫面為同一級別，簡單來說，此類型與另一項最大的區別，是在`BottomSheets`全部展開時，主畫面仍可以操作。
</br></br>
Ex: 可以在`app`中看到其應用場景(material.io)
</br>
<img src="bottom_sheets01.png" width=50% height=50% align=center/>

### Modal Bottom Sheet
通常用來取代`menu`或是`dialog`，此類型擁有比主畫面更高的高度`elevation`，也就是說，它並不是與主畫面處於同一級，而是像覆蓋在主畫面之上，與另一類最大的不同，就是他會阻止使用者與主畫面交互行為。
</br></br>
Ex: 可以在`app`中看到其應用場景(material.io)
</br>
<img src="bottom_sheets02.png" width=50% height=50% align=center/>
</br>
接下來來看看，如何在代碼中實現。

## Import Design Library

在`app`目錄下，`build.gradle`文件中，添加依賴庫

```
android {
	// ...       
}

dependencies {
   // ...
   implementation 'com.android.support:design:28.0.0'
}

```

## Add Bottom Sheets 
首先，我們來實現，如何在主畫面上，添加一個`BottomSheets`。

### Create Bottom Sheets Layout
在`layout`資源目錄下，創建一個新layout，並在`xml`的`ViewGroup`中，添加下面幾個屬性

1. behavior_hideable: 是否可以隱藏此視圖
2. behavior_peekHeight: 添加在主畫面時，未展開狀態下，保留的高度
3. layout_behavior: 定義了這個屬性，相當於告訴了`CoordinatorLayout `，這是一個`BottomSheets`，`@string/bottom_sheet_behavior`是定義在支持庫中的字符串，等效於`android.support.design.widget.BottomSheetBehavior`。

`bottom_sheets.xml`，如下

``` bash
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/bottom_sheet"
    ...
    app:behavior_hideable="true"
    app:behavior_peekHeight="48dp"
    app:layout_behavior="android.support.design.widget.BottomSheetBehavior">

   ...
   
</LinearLayout>
```

### Add Bottom Sheets to View
緊接著在主畫面中，添加`bottom_sheets.xml`，這邊要注意的一點是，`Bottom Sheets Behavior`是`CoordinatorLayout`的子類，所以在添加到主畫面時，務必注意主畫面的`ViewGroup`是否為`CoordinatorLayout `。更多`CoordinatorLayout`請參考另一篇文章[[MaterialDesign] Android CoordinatorLayout](http://nickcode4fun.net/2018/02/05/MaterialDesign-Android-CoordinatorLayout/)

主畫面`layout.xml`下添加，如下

``` bash
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <!-- Adding main view content -->
    <include layout="@layout/activity_main" />
    
    <!-- Adding bottom sheet after main content -->
    <include layout="@layout/bottom_sheet" />

</android.support.design.widget.CoordinatorLayout>
```

</br>

`activity_main.xml`佈局：

``` bash
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingEnd="16dp"
    android:paddingStart="16dp">

    <android.support.v7.widget.AppCompatButton
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="5dp"
        android:onClick="showBottomSheet"
        android:text="Bottom Sheet" />

</LinearLayout>
```

</br>
`MainActivity.kt`代碼如下:

``` bash
class MainActivity : AppCompatActivity() {

    lateinit var bottomBehavior: BottomSheetBehavior<View>
    lateinit var bottomSheet: View

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.layout)
        bottomSheet = findViewById<View>(R.id.bottom_sheet)
        bottomBehavior = BottomSheetBehavior.from(bottomSheet)
    }

    fun showBottomSheet(v: View) {
        setBottomViewVisible(bottomBehavior.state != BottomSheetBehavior.STATE_EXPANDED)
    }

    private fun setBottomViewVisible(isShow: Boolean) {
        if (isShow)
            bottomBehavior.state = BottomSheetBehavior.STATE_EXPANDED
        else
            bottomBehavior.state = BottomSheetBehavior.STATE_COLLAPSED
    }
}
```

<img src="bottom_sheets03.gif" width=50% height=50% align=center/>
</br>

## Control Persistent Bottom Sheets
我們透過取得`BottomSheets`的`Behavior`來控制`BottomSheets`的狀態。
</br>
`BottomSheets`總共有5種狀態

1. STATE_COLLAPSED: 默認折疊狀態，只顯示底部一小部分佈局，顯示高度可以通過`app:behavior_peekHeight`設置。
2. STATE_DRAGGING: 過度狀態，此時`BottomSheets`正在向上或向下拖動。
3. STATE_SETTLING: 手勢離開視圖，自由滑動到停止的一小段時間
4. STATE_EXPANDED: 佈局全展開狀態。
5. STATE_HIDDEN: 默認無此狀態(可通過：`app:behavior_hideable `啟用此狀態)，用戶能通過下向滑動，完全隱藏`BottomSheets`。

在代碼中實踐
``` bash
BottomSheetBehavior.setState(BottomSheetBehavior.STATE_COLLAPSED)；
```

## BottomSheetDialog
為`Modal Bottom Sheet`的一種，與`Persistent Bottom Sheets`最大的不同點就是會阻止，用戶與主畫面交互，接下來讓我們看看如何在代碼中實踐。

``` bash
	val view: View = layoutInflater.inflate(R.layout.bottom_sheet, null)
	val dialog: BottomSheetDialog = BottomSheetDialog(this)
	dialog.setContentView(view)
	dialog.show()
```

佈局與上面`layout.bottom_sheet.xml`相同。效果如下:
</br>
<img src="bottom_sheets04.gif" width=50% height=50% align=center/>
</br>

## BottomSheetDialogFragment
與`BottomSheetDialog`相同，會阻止主畫面的交互行為。實踐起來也很簡單。

### Extends BottomSheetDialogFragment
首先，創建一個class繼承`BottomSheetDialogFragment`
</br>

`BottomSheetFragment.kt`:

``` bash
class BottomSheetFragment : BottomSheetDialogFragment() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.bottom_sheet, container, false)
    }
}

```

佈局與上面`layout.bottom_sheet.xml`相同。接著在主畫面代碼中添加:

``` bash
val bottomSheetFragment = BottomSheetFragment()
bottomSheetFragment.show(supportFragmentManager, bottomSheetFragment.tag)
```

效果如下:
</br>
<img src="bottom_sheets05.gif" width=50% height=50% align=center/>
</br>
</br>
</br>
</br>
</br>
</br>
</br>

參考資料：
</br>
[androidhive](https://www.androidhive.info/2017/12/android-working-with-bottom-sheet/)
</br>
[MaterialDesign-Bottom Sheets](https://material.io/develop/android/components/bottom-sheet-behavior/)
</br>






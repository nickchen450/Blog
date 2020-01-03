---
title: '[MaterialDesign] Android Progress Bar'
date: 2018-01-17 13:33:51
tags: ['Android','Material Design']
---

## Introduction

**ProgressBar**是在Android常見的元件，尤其是在**materials design**上，依照 UI 上顯示的方式，可以初步分類成兩種<!-- more -->：
**1.Linear長條形**
**2.Circular圓型**
接下來我們來看看如何使用。

## Create a Circular Progress Bar

在**layout**文件下，直接引用**ProgressBar**

**layout:**
``` bash
 <ProgressBar
        android:id="@+id/progress_id"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>
```

運行效果如下：
<img src="progressbar01.gif" width=50% height=50% align=center/>

**ProgressBar**顏色預設是使用**@color/colorAccent**，想要改變顏色，需先創建一個新**style**，並自訂**colorAccent**的顏色

**styles.xml:**
``` bash
<resources>
   ...
  <style name="ProgressTheme" parent="Widget.AppCompat.ProgressBar">
        <item name="colorAccent">@color/colorDeepOrangeA200</item>
  </style>
</resources>
```

接著在**ProgressBar**添加屬性**android:theme**為我們自定義的新**style**
``` bash
android:theme="@style/ProgressTheme"
```

運行效果如下：
<img src="progressbar02.gif" width=50% height=50% align=center/>


## Create a Linear Progress Bar

**Linear Progress Bar** 依照顯示的型態，又可細分成4項
**1.Determinate(定值)**
	-有定值，顯示在進度條上，需要定義當前值與最大值
**2.Indeterminate(不定值)**
	-無定值，UI顯示上會從進度條開始，快速到結束，重複循環
**3.Buffer(緩衝)**
	-影片或加載圖片很常用到的緩衝進度條，通常是顯示在確定值的後方
**4.Query Indeterminate and Determinate (混合)**
	-結合 Determinate 和 Indeterminate 的進度條

創建一個**Linear Progress Bar**，方法很簡單，在剛剛創建的**ProgressBar**添加上一條**style**屬性，此時就會發現**ProgressBar**已經顯示為長條形
``` bash
style="@style/Widget.AppCompat.ProgressBar.Horizontal"
```

接著再加上一條屬性**android:indeterminate**，表示**ProgressBar**為不定值，會像** Circular Progress Bar ** 一樣，有循環的效果
``` bash
android:indeterminate="true"
```

運行效果如下：
<img src="progressbar03.gif" width=50% height=50% align=center/>

接下來我們來創建 ****Determinate(定值) ProgressBar****

先把屬性**android:indeterminate**，設為**false**
``` bash
android:indeterminate="false"
```

接著設定屬性進度值與最大值，Android會自動換算成百分比
``` bash
android:progress="20"
android:max="100"
```

當然也可以在代碼裡設定
``` java
progressBar.setProgress(20);
progressBar.setMax(100);
```

運行效果如下：
<img src="progressbar04.png" width=50% height=50% align=center/>

使用**Buffer(緩衝) ProgressBar**，需在**ProgressBar**裡添加屬性 **android:secondaryProgress**，就可以在畫面上，看見一條緩衝進度條效果
``` bash
android:secondaryProgress="40"
```

當然也可以在代碼裡設定
``` java
progressBar.setSecondaryProgress(40);
```

運行效果如下：
<img src="progressbar05.png" width=50% height=50% align=center/>


參考網站:
[Android ProgressBar](https://developer.android.com/reference/android/widget/ProgressBar.html)
[Material Design Progress & activity](https://material.io/guidelines/components/progress-activity.html)
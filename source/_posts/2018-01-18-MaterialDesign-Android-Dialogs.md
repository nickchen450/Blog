---
title: '[MaterialDesign] Android Dialogs'
date: 2018-01-18 14:20:24
tags: ['Android','Material Design']
---

## Introduction

在開發App的過程中，有一些情況下，你必須請使用者做一些決定，或是向使用者提醒一些錯誤，這時候就需要用到**Dialog**，在**Material Design**出現之前，**Dialog**的樣式的確不太美觀，接下來我們實際演示，如何使用具**Material Design Features**的**Dialog**<!-- more -->

如果你使用新版的**Android Studio 3.0**，專案預設會幫你載入**support:appcompat-v7**包，創建**AlertDialog**時，記得引用此包，不要用app內建的

使用系統內的**android.app.AlertDialog**在4.x版本，顯示效果如下：
5.0以上系統已經預設使用**Material Design**的風格
<img src="dialogs01.png" width=100% height=100% align=center/>

如果使用**android.support.v7.app.AlertDialog**，在4.x版本下，也能有與5.0以上樣式相似的效果
<img src="dialogs02.png" width=100% height=100% align=center/>

## Alert Dialog
常用於錯誤通知、或提示使用者接下來行為/動作

### Create an Alert Dialog
接下來我們用代碼來創建一個**AlertDialog**

``` java
new AlertDialog.Builder(this)
        .setMessage("Discard draft?")
        .create()
        .show();
```

運行效果如下：
<img src="dialogs03.png" width=50% height=50% align=center/>

使用**AlertDialog**通常會搭配使用兩個按鈕(flat button)，右邊按鈕通常放置**確定/同意**...等動作，左邊按鈕通常放置**取消/不同意**...等動作

``` java
new AlertDialog.Builder(this)
        .setMessage("Discard draft?")
        .setPositiveButton("DISCARD", null) //創建右邊按鈕，第二個參數 listener 可以監聽點擊事件
        .setNegativeButton("CANCEL", null) //創建左邊按鈕，第二個參數 listener 可以監聽點擊事件
        .create()
        .show();
```

運行效果如下：
<img src="dialogs04.png" width=50% height=50% align=center/>

### Customize the Alert Dialog
改變按鈕的顏色，有兩種方法，第一種是直接在代碼裡定義兩個按鈕的顏色
``` java
alertDialog = new AlertDialog.Builder(this)
                .setMessage("Discard draft?")
                .setPositiveButton("DISCARD", null) //創建右邊按鈕，第二個參數 listener 可以監聽點擊事件
                .setNegativeButton("CANCEL", null) //創建左邊按鈕，第二個參數 listener 可以監聽點擊事件
                .create();

alertDialog.show();

// 取得右邊按鈕，改變文字顏色
alertDialog.getButton(AlertDialog.BUTTON_POSITIVE).setTextColor(ContextCompat.getColor(this, R.color.colorLightBlue));
```

運行效果如下：
<img src="dialogs05.png" width=50% height=50% align=center/>

可以看到右邊的文字顏色已經改變，如果還需要改變左邊按鈕文字顏色可以加入下方代碼
``` java
alertDialog.getButton(AlertDialog.BUTTON_NEGATIVE).setTextColor(ContextCompat.getColor(this, R.color.colorLightBlue));
```

第二種方法是創建一個新**style**，在**style**裡改變**colorAccent**顏色，接著使用**AlertDialog.Builder**的**constructor**創建有自定義**style**的**Dialog**
點進**AlertDialog.Builder**的代碼，可以發現多態**constructor**
``` java
package android.support.v7.app;
...
public class AlertDialog extends AppCompatDialog implements DialogInterface {
	...
	public Builder(@NonNull Context context){...}
	public Builder(@NonNull Context context, @StyleRes int themeResId){...}
}
```

在**styles.xml**裡，是創建一個新**style**，並在**style**裡自訂**colorAccent**顏色
``` bash
<style name="AlertDialogTheme" parent="Theme.AppCompat.Light.Dialog.Alert">
    <item name="colorAccent">@color/colorLightBlue</item>
</style>
```

接著回到代碼裡，在創建**AlertDialog**同時，把剛剛自訂的**style**添加進去
``` java
 new AlertDialog.Builder(this, R.style.AlertDialogTheme)// 添加style
        .setMessage("Discard draft?")
        .setPositiveButton("DISCARD", null) //創建右邊按鈕，第二個參數 listener 可以監聽點擊事件
        .setNegativeButton("CANCEL", null) //創建左邊按鈕，第二個參數 listener 可以監聽點擊事件
        .create()
        .show();
```

運行效果如下：
<img src="dialogs06.png" width=50% height=50% align=center/>

## Confirmation dialogs
確認對話框，常用在尋求使用者做一個行為或是動作的選擇時使用，**Confirmation dialogs**通常包含標題、選擇內容、確認/取消按鈕
<img src="dialogs07.png" width=50% height=50% align=center/>

### Create a Confirmation Dialog
接下來在代碼裡創建一個**Confirmation Dialog**範例
``` java
// dialogs 裡面的選項
String[] items = {"None", "Callisto", "Ganymede", "Luna"};
// 創建一個 confirmation dialog
new AlertDialog.Builder(this)
        .setTitle("Phone Ringtone")
        // 設置單選選項，第二個參數可以設置默認選擇，-1表示默認無選擇
        .setSingleChoiceItems(items, -1, new DialogInterface.OnClickListener() {
             @Override
             public void onClick(DialogInterface dialog, int which) {
                // 這邊監聽點擊事件
             }
        })
        .setPositiveButton("OK", null) //創建右邊按鈕，第二個參數 listener 可以監聽點擊事件
        .setNegativeButton("CANCEL", null) //創建左邊按鈕，第二個參數 listener 可以監聽點擊事件
        .create()
        .show();
```

運行效果如下：
<img src="dialogs08.png" width=50% height=50% align=center/>

### Customize the Confirmation Dialog
更換**style**樣式很簡單，與上面**AlertDialog**第二種方式相同，值得注意的是，套用**style後**，**dialog**裡面的內容，也會有改變

運行效果如下：
<img src="dialogs09.png" width=50% height=50% align=center/>

## ProgressDialog
如果你還在用**ProgressDialog**，那你可要小心了，因為**Android**官網已經在**API level 26**時棄用它，官方的解釋是**ProgressDialog**是一個模態對話框(modal dialog)，它會禁止使用者與App的交互，作為替代方案，可以在**App UI**裡嵌入**ProgressBar**作為替代方案，如果想實現對話框的模式，亦可在**AlertDialog裡嵌入ProgressBar來實現**

在**AlertDialog**中嵌入含有**ProgressBar**的**layout**

這邊很簡單就不附上xml了
layout:
<img src="dialogs10.png" width=50% height=50% align=center/>

在代碼裡創建**AlertDialog**，並把**ProgressBar**添加進去
``` java
 new AlertDialog.Builder(this, R.style.AlertDialogTheme)
        .setTitle("Progress Dialog")
        .setView(R.layout.layout_progress_bar)
        .create()
        .show();
``` 

運行效果如下：
<img src="dialogs11.gif" width=50% height=50% align=center/>

也可以使用**Linear Progress Bar**
<img src="dialogs12.gif" width=50% height=50% align=center/>

關於**ProgressBar**更多細節，請參考[[MaterialDesign] Android Progress Bar](http://nickcode4fun.net/2018/01/17/MaterialDesign-Android-Progress-Bar/)


參考網站:
[Android AlertDialog](https://developer.android.com/reference/android/app/AlertDialog.html)
[Android ProgressDialog](https://developer.android.com/reference/android/app/ProgressDialog.html)
[Android Dialogs](https://developer.android.com/guide/topics/ui/dialogs.html?hl=zh-tw)
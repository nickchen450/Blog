---
title: '[Material Design] Android Toolbar(三) - Add-Action'
date: 2017-11-26 13:57:50
tags: ['Android','Material Design']
---

## Introduction

除了 Pop-up Menu 外，我們還可以在 Toolbar 上自定義 action <!-- more -->例如：
<img src="addAction01.png" width=50% height=50% align=center/>

## Add Action to Toolbar

1.首先我們需要取得我們要顯示的 icon ，你可以在 [Material Design](https://material.io/guidelines/) 官網上尋找，或是在 [Material Design icon](https://materialdesignicons.com/) 上，尋找符合你需求且具有 Materical style 的 icon

2.接下來把下載下來的 icon 放在根目錄下 res -> drawable 目錄下，這邊比較需要注意的地方，要放在 Toolbar 的 action icon 尺寸最好為 48 或是 24，更詳細資訊請參考 [Material icons](https://material.io/guidelines/style/icons.html#)

3.接下來進到 res -> menu 目錄下，打開我們新增 Pop-up Menu 的 XML 檔，在下面新增一個 item tag

``` bash
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    ...

    <item
        android:id="@+id/item1_id"
        android:title="@string/main_item1" />
    <item
        android:id="@+id/item2_id"
        android:title="@string/main_item2" />
    <item
        android:id="@+id/item3_id"
        android:title="@string/main_item3" />

    <item
        android:id="@+id/search_id"
        android:title=""
        android:icon="@drawable/ic_search_white_24dp"
        app:showAsAction="always"/>

    ...

</menu>
```

因為我們要顯示的是 icon 所以 title 不需要，留空，icon 使用我們剛剛下載放在 drawable 資料夾下的 icon ，showAsAction 屬性設定，有分為三種
	1.always 是強制顯示 icon
	2.ifRoom 是如果還有空間的話就顯示，沒有就隱藏在 pop-up menu 裡
	3.never 是隱藏，只顯示在 Pop-up Menu 中

比較值得注意的是，如果你的 action 有超過一個以上，系統會建議你使用 ifRoom 在空間不夠的時候，會改顯示在 Pop-up Menu(ifRoom/never) 中，那你最好也把 title 加上去。

效果如下
<img src="addAction01.png" width=50% height=50% align=center/>

如果你設定 ifRoom 在空間不足或是 設定 never 時，效果如下
<img src="addAction02.png" width=50% height=50% align=center/>

4.如果仔細觀察市面上的 App 通常在 Toolbar 的左側，只會有一個按鈕，其中一種是用來返回上一頁，或結束此頁，接下來我們來新增左邊的 Action。
回到Activity中，在onCreate()的方法中，呼叫supporActionBar的setDisplayHomeAsUpEnabled()方法，將其設定為 true

``` bash
public class MainActivity extends AppCompatActivity {
	...

	@Override
	protected void onCreate(Bundle saveInstanceState) {
		super.onCreate(saveInstanceState)
		setContentView(R.layout.activity_main);
		...
		setSupportActionBar(toolbar);
		getSupportActionBar().setDisplayHomeAsUpEnabled(true);
	}

}
```

左側的返回鍵按鈕就跑出來了
<img src="addAction03.png" width=50% height=50% align=center/>

## Add On-Click Listener to the Action

在監聽點擊事件方面，原則上與 Pop-up Menu 相同，可以參考上一篇 [Material Design] Android Toolbar(二) - Pop-up Menu。
特別的一點是，如果我們想新增左側返回鍵按鈕的 function，我們同樣也是回到 Activity 的 onOptionsItemSelected() 中做判斷

``` bash
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    switch (item.getItemId()) {
    	...
    	// 左側返回鍵的 id
    	case android.R.id.home:
    		finish();
    		break;
    }
    return super.onOptionsItemSelected(item);
}
```

參考網站：
[Material icons](https://material.io/guidelines/style/icons.html#)
[Android Adding and Handling Actions](https://developer.android.com/training/appbar/actions.html)
[Material Design](https://material.io/guidelines/)
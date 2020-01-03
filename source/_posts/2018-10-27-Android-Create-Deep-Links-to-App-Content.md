---
title: '[Android] Create Deep Links to App Content'
date: 2018-10-27 16:13:33
tags: ['Android']
---

## Introduction
何謂`Deep Links`:
</br>

`Deep Links`是指透過指定連結，來喚起指定應用，並可以向指定頁面傳送數據，它使我們應用程式之間產生了關係，讓應用不再孤立，優化了使用者體驗。<!-- more -->
<br>

`Deep Links`採用`Uri Scheme`的方式來實現，如果不太了解`Uri Scheme`的人，可以查詢
[維基百科](https://zh.wikipedia.org/wiki/%E7%BB%9F%E4%B8%80%E8%B5%84%E6%BA%90%E6%A0%87%E5%BF%97%E7%AC%A6)
</br>
下面就簡單展示一下`Uri Scheme`組成:
</br>

```
   hierarchical part
        ┌───────────────────┴─────────────────────┐
                    authority               path
        ┌───────────────┴───────────────┐┌───┴────┐
  abc://username:password@example.com:123/path/data?key=value&key2=value2#fragid1
  └┬┘   └───────┬───────┘ └────┬────┘ └┬┘           └─────────┬─────────┘ └──┬──┘
scheme  user information     host     port                  query         fragment

  urn:example:mammal:monotreme:echidna
  └┬┘ └──────────────┬───────────────┘
scheme              path
```

</br>
下面我們來實做一下，如何在`Android`上實現`Deep Links`。

## Add intent filters for incoming links
首先當我們在點擊連結或 Web URI 的意圖時，`Android`系統會依序執行以下操作，直到意圖成功被`Handle`
</br>

1. 使用者指定的可以處理`URI`的應用程式</br>
2. 打開安裝的應用中，可以處理此`URI`的應用</br>
3. 允許用戶選擇可以Handle此`URI`的應用程式</br>

所以為了讓`URI`成功被`Handle`，我們必須在`Android`專案目錄裡`manifest`文件下，指定`activity`標籤中，新增`intent-filter`

```
 <application...>
 	
 	<activity ...>
 	
 		<intent-filter>
			<!--指定Action View的意圖操作，此intent-filter才可以從Google Search被搜尋-->
			<action android:name="android.intent.action.VIEW" />
			
			<!--類別需包含BROWSABLE，Web瀏覽器才可以訪問intent-filter-->
			<category android:name="android.intent.category.BROWSABLE" />
			<!--還必須包含DEFAULT，此允許應用可以響應隱式意圖，讓URI被過濾時啟動你的應用-->
			<category android:name="android.intent.category.DEFAULT" />
			
			<!--data表示每一個解析URI的格式，可以添加多個解析格式，但每一個最少要包含一個scheme屬性-->
			<!-- Accepts URIs that begin with "http://nickcode4fun.net/archives” -->
			<data
				android:scheme="http"
				android:host="nickcode4fun.net"
				android:pathPrefix="/archives" />
			
			...
			<data.../>
				
		</intent-filter>
 	
	</activity> 
	
 </application>
```

## Read data from incoming intents
你可以在系統啟動你的`Activity`時，透過`Intent`中的`getData()`與`getAction()`方法，取得與意圖相關的資訊。

```
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.main);

    Intent intent = getIntent();
    String action = intent.getAction();
    Uri data = intent.getData();
}
```
運行效果如下:
</br>
<img src="deep_link01.gif" width=50% height=50% align=center/>

那麼如何取得`URL`攜帶的`Data`呢？
</br>
Ex: `http://nickcode4fun.net/archives?type=1&id=10001`
</br>

```
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.main);

     Intent intent = getIntent();
     String action = intent.getAction(); 
     Uri uri = intent.getData();
     assert uri != null;
     String scheme = uri.getScheme();
     String host = uri.getHost();
     String path = uri.getPath();
     String query = uri.getQuery();
     String type = uri.getQueryParameter("type");
     String id = uri.getQueryParameter("id");

     final String msg = "action: " + action + "\n"
                 + "uri: " + uri + "\n"
                 + "scheme: " + scheme + "\n"
                 + "host: " + host + "\n"
                 + "path: " + path + "\n"
                 + "query: " + query + "\n"
                 + "type: " + type + "\n"
                 + "id: " + id + "\n";

}
```

運行效果如下:
</br>
<img src="deep_link02.png" width=50% height=50% align=center/>
</br>
</br>
現在`Android Studio`提供圖形化介面來管理`Deep Links`，詳情可以參考下面連結

1. Select Tools > App Links Assistant.
2. Click Open URL Mapping Editor and then click Add   at the bottom of the URL Mapping list to add a new URL mapping.
</br>
<img src="deep_link03.png" width=100% height=100% align=center/>
</br>
</br>
</br>
</br>
</br>
</br>

參考資料：
</br>
[Create Deep Links to App Content](https://developer.android.com/training/app-links/deep-linking#java)
</br>
[Add Android App Links](https://developer.android.com/studio/write/app-link-indexing)
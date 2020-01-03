---
title: '[Material Design] Android Navigation Drawer(二) - Add OnClickListener'
date: 2017-12-30 14:59:16
tags: ['Android','Material Design']
---

## Introduction

接下來我們在點擊 Navigation Drawer item 時，加入點擊事件<!-- more -->

### Implement the method

首先我們先在代碼裡，呼叫 setNavigationItemSelectedListener 方法，去添加 Listener，接下來在 activity class 下實現 NavigationView.OnNavigationItemSelectedListener 接口，並覆寫 onNavigationItemSelected 方法

``` bash
public class MainActivity extends AppCompatActivity implements NavigationView.OnNavigationItemSelectedListener {

    Toolbar toolbar;
    DrawerLayout drawerLayout;
    NavigationView navigationView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.navigation_drawer);

        toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        drawerLayout = findViewById(R.id.drawer_layout);
        navigationView = findViewById(R.id.navigation_view);

        //這邊添加 NavigationItemSelectedListener
        navigationView.setNavigationItemSelectedListener(this);
        
        ActionBarDrawerToggle toggle = new ActionBarDrawerToggle(this, drawerLayout, toolbar, R.string.open_drawer, R.string.close_drawer);
        drawerLayout.addDrawerListener(toggle);
        toggle.syncState();
    }

    @Override
    public boolean onNavigationItemSelected(@NonNull MenuItem item) {
    	// 實現此方法
        return false;
    }
}
```

### Add function
接下來在 onNavigationItemSelected 方法中，把return 改為 true，並在方法裡取出 MenuItem 的 id 以 switch 判別哪個 item 被點擊。在 switch 部份結束後，我們呼叫 drawerLayout closeDrawer() 方法，表示在點擊事件結束後，會關閉 drawer。

``` bash
public class MainActivity extends AppCompatActivity implements NavigationView.OnNavigationItemSelectedListener {

    ...

    @Override
    public boolean onNavigationItemSelected(@NonNull MenuItem item) {
        int id = item.getItemId();

        switch (id) {
            case R.id.inbox_id:
            	// 這邊新增要呼叫的方法
                Toast.makeText(getApplicationContext(), "inbox", Toast.LENGTH_LONG).show();
                break;
            
            case R.id.starred_id:
                Toast.makeText(getApplicationContext(), "starred", Toast.LENGTH_LONG).show();
                break;
            
            case R.id.sent_id:
                Toast.makeText(getApplicationContext(), "sent mail", Toast.LENGTH_LONG).show();
                break;
            
            case R.id.drafts_id:
                Toast.makeText(getApplicationContext(), "drafts", Toast.LENGTH_LONG).show();
                break;
            
            case R.id.allmail_id:
                Toast.makeText(getApplicationContext(), "all mail", Toast.LENGTH_LONG).show();
                break;
            
            case R.id.trash_id:
                Toast.makeText(getApplicationContext(), "trash", Toast.LENGTH_LONG).show();
                break;
            
            case R.id.spam_id:
                Toast.makeText(getApplicationContext(), "spam", Toast.LENGTH_LONG).show();
                break;
        }
        // 關閉 drawer，GravityCompat.START 表示 drawer 關閉會回到螢幕的左方
		drawerLayout.closeDrawer(GravityCompat.START);
        return true;
    }
}
```

運行效果如下:
<img src="navigationDrawerFunction01.gif" width=50% height=50% align=center/>

參考網站:
[Android Developer](https://developer.android.com/training/implementing-navigation/nav-drawer.html)
[Material Design](https://material.io/guidelines/patterns/navigation-drawer.html)

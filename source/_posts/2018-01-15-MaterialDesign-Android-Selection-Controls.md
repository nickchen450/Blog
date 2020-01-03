---
title: '[MaterialDesign] Android Selection Controls'
date: 2018-01-15 16:15:57
tags: ['Android','Material Design']
---

## Introduction

**Selection Controls**是在**Android**上很常見的元件，這些控制元件包括 **Switches、CheckBoxes、Radio buttons**<!-- more -->接下來我們使用**support.v7.widget**包下的元件，來看看這些具有**Material Features**的元件怎麼使用，這邊值得注意的一點是，**support.v7.widget**包下的元件，要顯示**Material Features**需在**Android**版本**Lollipop**(5.0)之後的版本。

## Create a SwitchCompat

首先在**layout**文件下，引用**support.v7.widget**包下的**SwitchCompat**

**layout:**
``` bash
 <android.support.v7.widget.SwitchCompat
       android:id="@+id/switch_id"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:text="@string/switch_off_on"/>
```

運行效果如下：
<img src="switchcompat01.gif" width=50% height=50% align=center/>

**SwitchCompat**預設的狀態都是**false**，想要改變預設狀態，只要在**SwitchCompat**添加屬性**android:checked**就行了
``` bash
 android:checked="true"
```

**SwitchCompat**的顏色預設是使用**@color/colorAccent**，想要改變顏色，需先創建一個新**style**，並自訂**colorAccent**的顏色

**styles.xml:**
``` bash
<resources>
   ...
   <style name="SwitchTheme">
        <item name="colorAccent">@color/colorTealA200</item>
    </style>
</resources>
```

接著在**SwitchCompat**添加屬性**app:theme**為我們自定義的新**style**
``` bash
app:theme="@style/SwitchTheme"
```

運行效果如下：
<img src="switchcompat02.gif" width=50% height=50% align=center/>

在程式碼裡監聽點擊事件
``` java
switchCompat.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                // 這邊監聽點擊事件
            }
        });
```

## Create an AppcompatCheckBox

在**layout**文件下，引用**support.v7.widget**包下的**AppcompatCheckBox**

**layout:**
``` bash
<android.support.v7.widget.AppCompatCheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/checkbox_id"
        android:text="@string/checkbox"/>
```

運行效果如下：
<img src="checkbox01.gif" width=50% height=50% align=center/>

改變預設狀態，只要在**AppcompatCheckBox**添加屬性**android:checked**就行了
``` bash
 android:checked="true"
```

**AppcompatCheckBox**的顏色
1.狀態**true**時，使用**colorAccent**
2.狀態**false**時，使用**colorControlNormal**
改變顏色，需要創建一個新**style**，並在裡面定義兩個**item**的顏色

**styles.xml:**
``` bash
<resources>
   ...
    <style name="CheckBoxTheme">
        <item name="colorAccent">@color/colorTealA200</item>
        <item name="colorControlNormal">@color/colorDeepOrangeA200</item>
    </style>
</resources>
```

接著在**AppcompatCheckBox**添加屬性**app:theme**為我們自定義的新**style**
``` bash
app:theme="@style/CheckBoxTheme"
```

運行效果如下：
<img src="checkbox02.gif" width=50% height=50% align=center/>

在程式碼裡監聽點擊事件
``` java
checkBox.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                // 這邊監聽點擊事件
            }
        });
```

## Create an AppCompatRadioButton
**AppCompatRadioButton** 比較值得注意一點是，它只要**state = true**後，就無法透過點擊設為**false**
在**layout**文件下，引用**support.v7.widget**包下的**AppCompatRadioButton**

**layout:**
``` bash
<android.support.v7.widget.AppCompatRadioButton
        android:id="@+id/radioButton1_id"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/radiobutton_1"/>
```

運行效果如下：
<img src="radiobutton01.png" width=70% height=70% align=center/>

改變預設狀態，只要在**AppCompatRadioButton**添加屬性**android:checked**就行了
``` bash
 android:checked="true"
```

**AppCompatRadioButton**的顏色預設是使用**@color/colorAccent**，想要改變顏色，首先需要創建一個新**style**，自訂**colorAccent**的顏色

**styles.xml:**
``` bash
<resources>
   ...
   <style name="RadioButtonTheme">
        <item name="colorAccent">@color/colorTealA200</item>
    </style>
</resources>
```

接著在**SwitchCompat**添加屬性**app:theme**為我們自定義的新**style**
``` bash
app:theme="@style/RadioButtonTheme"
```

運行效果如下：
<img src="radiobutton02.png" width=70% height=70% align=center/>

在程式碼裡監聽點擊事件
``` java
radioButton.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                // 這邊監聽點擊事件
            }
        });
```

### Add a RadioGroup to the AppcompatRadioButton

如果我們想要創建一排**AppcompatRadioButton**列表，並保持列表中，只有一個**state = true**(被選中)，其他為**false**，那麼就需要使用**RadioGroup**
**RadioGroup**使用起來很簡單，只要引用在**AppcompatRadioButton**外層就行了

**layout:**
``` bash
<RadioGroup
        android:id="@+id/radioGroup_id"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content">

        <android.support.v7.widget.AppCompatRadioButton
            android:id="@+id/radioButton1_id"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/radiobutton_1" />

        <android.support.v7.widget.AppCompatRadioButton
            android:id="@+id/radioButton2_id"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/radiobutton_2" />

        <android.support.v7.widget.AppCompatRadioButton
            android:id="@+id/radioButton3_id"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/radiobutton_3" />

    </RadioGroup>
```

運行效果如下：
<img src="radiogroup01.gif" width=50% height=50% align=center/>

在程式碼裡監聽點擊事件
``` java
radioGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup group, int checkedId) {
                // 這邊監聽點擊事件
            }
        });
```



參考網站:
[Android SwitchCompat](https://developer.android.com/reference/android/support/v7/widget/SwitchCompat.html)
[Android AppCompatCheckBox](https://developer.android.com/reference/android/support/v7/widget/AppCompatCheckBox.html)
[Android AppCompatRadioButton](https://developer.android.com/reference/android/support/v7/widget/AppCompatRadioButton.html)
[Android RadioGroup](https://developer.android.com/reference/android/widget/RadioGroup.html)
[Material Design Selection controls](https://material.io/guidelines/components/selection-controls.html)




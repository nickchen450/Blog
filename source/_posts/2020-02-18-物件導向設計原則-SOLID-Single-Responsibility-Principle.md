---
title: 物件導向設計原則(SOLID)#1 - Single Responsibility Principle
date: 2020-02-18 14:35:52
tags:
---

物件導向設計本身就具有`封裝`、`繼承`、`多型`、`抽象`這些特性。

那麼是否掌握這些特性，就可以寫出具備`閱讀性`、`維護性`、`擴充性`的程式碼呢？
<!-- more -->
非也...
<div align="center">
    <img src="still_too_young.gif" width=30% height=30% align=center>
</div>

</br>
</br>

在物件導向開發軟體的過程中，遵守`Robert C. Martin`提出的物件導向設計的五個原則(`SOLID`)。這樣會更容易開發出易維護與可擴展的系統。


縮寫     |   英文全名                           | 中譯
--------|:-------------------------------:|------------
SRP     |   Single Responsibility Principle |   單一職責原則
OCP     |   Open Closed Principle           |   開放封閉原則
LSP     |   Liskov Substitution Principle   |   里氏替換原則
ISP     |   Interface Segregation Principles|   介面隔離原則
DIP     |   Dependency Inversion Principle  |   依賴反轉原則

</br>

### Single Responsibility
對於不熟的開發者，很常犯的錯誤就是把很多功能寫在同一個 class，雖然讓這個 class 擁有許多功能，但也大大增加了維護的困難。整體來說`Z>B`(利大於弊)。

那麼要做的是很簡單，讓 class 一次只做一件事，並移除與這件事情無關的程式碼和變數。

> Do one thing and do it well.

#### Example

``` java
public class StockReporter {

    /**
     * 查詢股票更新時間
     *
     * @param symbol 股市代號
     */
    public String getStockUpdateTime(String symbol) {
        Stock stock = queryStockFromServer(symbol);
        return stock.getTime();
    }

    /**
     * 從服務器取得股票資料
     *
     * @param symbol 股市代號
     */
    public Stock queryStockFromServer(String symbol) {
        // 模擬從 API 取得資料
        return new Stock(
                "ATA CREATIVITY GLOBAL - ADR",
                "AACG",
                1.16f,
                "2020-02-18");
    }
}
```

這個範例很明顯違反了 Single Responsibility 原則
> 要改變資料取得方式，必須修改這個 Class

#### Solution

##### 改變資料取得方式

``` java
/**
* 從服務器取得股票資料
*
* @param symbol 股市代號
*/
public Stock queryStockFromServer(String symbol) {
    // 模擬從 API 取得資料
    return new Stock(
            "ATA CREATIVITY GLOBAL - ADR",
            "AACG",
            1.16f,
            "2020-02-18");
}
```

##### 使用 Repository 來取得資料

``` java
 class Repository {
        /**
         * 從服務器取得股票資料
         *
         * @param symbol 股市代號
         */
        public Stock queryStockFromServer(String symbol) {
            // 模擬從 API 取得資料
            return new Stock(
                    "ATA CREATIVITY GLOBAL - ADR",
                    "AACG",
                    1.16f,
                    "2020-02-18");
        }
    }
```
因此，`StockReporter` 修改如下

``` java
public class StockReporter {

    private Repository repository;

    public StockReporter(Repository repository) {
        this.repository = repository;
    }

    /**
     * 查詢股票更新時間
     *
     * @param symbol 股市代號
     */
    public String getStockUpdateTime(String symbol) {
        Stock stock = repository.queryStockFromServer(symbol);
        return stock.getTime();
    }
}
```

讓取得資料的相依關係透過 `Repository` 來處理

如此一來，當我們要更改取得資料這段邏輯的時候，便不需要更改 `StockReporter` 了。

最後 `StockReporter` 使用方式如下：

``` java
 public void use() {
    StockReporter reporter = StockReporter(new Repository();
    
    String updateTime = reporter.getStockUpdateTime("AACG");
    
    System.out.println("AACG update time: " + updateTime);    
}
```
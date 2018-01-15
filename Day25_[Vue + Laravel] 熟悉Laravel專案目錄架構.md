# Day25 - [Vue + Laravel] 熟悉Laravel專案目錄架構

上一篇我們已經將Laravel專案建立出來了，因為我習慣在建立一個framework專案後，會先認識一下其專案的架構，以便之後開發上較能了解哪個目錄是放入什麼檔案，所以這篇我們來稍微熟悉一下Laravel專案的架構與它是怎麼運作的。

## Laravel專案架構

當我們在終端機輸入以下指令，就會建立出一個完整的Laravel專案了。

```
$ composer create-project --prefer-dist laravel/laravel [Project_name]
```

一般來說，我們建立的Laravel專案，它的架構會如下圖所示：

<img src="picture/25_laravel_architecture.png">

接下來我們細講比較重要的目錄底下放入的資料夾或文件類別：

### 1. `app`目錄

#### 專案建立好即預設出現的資料夾與檔案

* `Console`：含所有自定義的Artisan指令與控制台內核(Kernel.php)。指令可透過`make:command`來生成，而Kernel.php可用來註冊自定義的Artisan指令與任務排程。
* `Exceptions`：含此App的例外處理程序，可在Handler.php自定義例外處理的方式。
* `Http`：含控制器(Controller)、中介層(Middleware)和表單請求，此App的所有的請求處理都放在此資料夾底下。
* `Providers`：含此App所有的服務提供器。

> `Console`與`Http`提供API進入App的「核心」，它們不包含應用程式邏輯，是兩種發布命令給應用程式的方法。

#### 一些常用但預設不存在的資料夾，須透過Artisan指令後才會產生

* `Events`：執行`event:generate`或`make:event`生成，主要拿來存放事件類別，用於當使用者觸發行為時，會通知App其他部分。
* `Jobs`：執行`make:job`生成，主要存放此App的可隊列任務。
* `Listeners`：執行`event:generate`或`make:listener`生成，含事件的處理類別，當事件監聽器接受到一個事件，會觸發該事件的處理邏輯程序。

> 除了上面介紹的三個目錄，`app`底下有很多目錄是可以透過Artisan指令產生的，我們可以在終端機輸入並執行`php artisan list make`，就可以查看哪些指令可以使用了。

### 2. `bootstrap`目錄

* `cache`：含一些框架對啟動效能最佳化所產生的檔案。

### 3. `database`目錄

* `migrations`：含資料庫遷移檔案，也就是資料庫操作的相關資料皆存放在這目錄之下。
* `seeds`：含設定自動填入資料庫的資料(假資料)產生的檔案。

### 4. `resources`目錄

* `assets/js/components`：看到components，是不是有些熟悉，此目錄下可放入`.vue`檔，給Laravel去編譯，裡面有一個預設好的`ExampleComponent.vue`檔案。
* `lang`：存放多國語言資料。
* `views`：存放每個頁面的樣板。

### 5. `routes`目錄

* `api.php`：包含`RouteServiceProvider`，因為路由是無狀態的，所以在這些api路由通過App請求時需進行身份認證，提供頻率限制。
* `channels.php`：註冊App支持的所有事件推播器。
* `console.php`：定義基於閉包(closure)的控制台命令。
* `web.php`：除了無狀態的路由，幾乎此App所有的路由都在這邊定義，包含`RouteServiceProvider`，提供web之間的對話狀態、CSRF防護和cookie加密。

### 6. `storage`目錄

* `app`：存放此App使用的任何檔案。
* `framework`：存放儲存框架產生的檔案與快取。
* `logs`：存放此App的日誌檔案。

以上我們大致了解整個Laravel的專案架構了，整個App運作的入口點是在根目錄下的`public`資料夾裡面的`index.php`，其實每個framework產生出的架構都大同小異，下一篇我們將介紹在Laravel專案中要如何放入Vue檔並且讓Laravel去編譯樣板，顯示出我們想要的畫面。

-----

### 參考資料
* [Laravel 的文件夹结构](https://d.laravel-china.org/docs/5.5/structure#the-root-app-directory)
* [Laravel 框架目錄說明](https://tony915.gitbooks.io/laravel4/content/install/laravel_framework.html)
* [PHP Laravel 開發入門(二) - 設置與目錄架構](http://www.codedata.com.tw/uncategorized/php-laravel-dev-tutorial-2-class-configuration-structure)
# Day10 - [Directives] 事件處理(Event Handling)

我們看到的網頁，了解使用者對UI元件的操作，不會只有是在表單元素內輸入或選擇內容，還有點擊按鈕或送出表單等等**DOM事件**，而事件的觸發會造成資料的異動，為了得知資料異動的前後差異，並且做出相對應的回應，才能完成一次的事件處理，而且必須即時**監聽**是否有事件被觸發，整個網頁才會有互動性。

所以這篇我們將介紹Vue的**事件處理**(Event Handling)，先來回憶一下前面我們提到Vue的MVVM model：

<img src="picture/2_mvvm.png">

從上圖可以了解到，Vue可以將**事件監聽**放在HTML元素中，再透過ViewModel將事件處理方法(method)與data綁定在View上，使用者看到的UI就會有剛剛處理好的事件處理結果，如此一來，開發者也可以很容易找到事件監聽器對應的事件處理方法。

那首先，我們先認識一下選項物件(Options)的一個屬性，`methods`(方法)。

## 選項物件屬性：`methods`

`methods`這個屬性用來定義方法，如果我們在UI操作了什麼動作，都可以寫`methods`去回傳方法，執行相對應的事件，通常`methods`定義方法的方式是一個包住`function`的`object`。下面我們寫一個方法可以用來計數，範例如下：

```html
<div id="app">
    <p>{{ count }}</p>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        count: 0
    },
    methods: {
        addCount: function() {
            this.count += 1;
        }
    }
})
```

為了增加使用者操作頁面的互動性，所以當方法寫好後，就需要有個地方去呼叫(觸發)這個方法，此時我們可以在HTML元素中加入事件監聽器，使用的指令就是`v-on`。

## 事件處理指令

### `v-on`

* 用途：在HTML元素中加入事件監聽器，即可找到對應的事件處理方法，也就是我們在vue instance宣告的methods。
* 縮寫：`@`
* 接受值：`function`、`inline statement`、`object`



-----

### 參考資料
* [Vue.js - v-on](https://cn.vuejs.org/v2/api/#v-on)
* [Vue.js: Methods 與事件處理 (Event Handling)](https://cythilya.github.io/2017/04/17/vue-methods-and-event-handling/)
* [Vue.js (4) - 事件處理指令](http://blog.tonycube.com/2017/04/vuejs-4-event.html)
* [Vue.js 11 - DOM事件處理](https://ithelp.ithome.com.tw/articles/10187655)

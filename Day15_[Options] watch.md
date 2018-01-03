# Day15 - [Options] watch

在做事件處理的時候，當事件被觸發，資料異動了，通常這時候我們會需要**監聽器**去聽到觸發事件的啟動並去呼叫對應的處理器，並讓它做事情。

今天我們將介紹Vue在做事件處理的時候，一個扮演重要角色的options屬性：`watch`。

## `watch`(監聽器)

* 用途：監聽某個data值(變數)，當這個值變動時，就會去做對應的事情(函數)。
* 用法：

下面我們應用`v-model`與`watch`舉一個範例來寫時間單位的轉換。

```html
<div id="app">
    <p>g(公克)：<input type="text" v-model="g"></p>
    <p>kg(公斤)：<input type="text" v-model="kg"></p>
    <p>t(公噸)：<input type="text" v-model="t"></p>
</div>
```

```javascript
var vm = new Vue ({
    el: '#app',
    data: {
        g: 0,
        kg: 0,
        t: 0
    },
    watch: {
        g: function(value) {
            this.g = value;
            this.kg = value / 1000;
            this.t = value / 1000 / 1000;
        },
        kg: function(value) {
            this.g = value * 1000;
            this.kg = value;
            this.t = value / 1000;
        },
        t: function(value) {
            this.g = value * 1000 * 1000;
            this.kg = value * 1000;
            this.t = value;
        }
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/hbo9oy8f/)

## `watch` vs `computed`

### `watch`
* 觀察特定的值
* 適用時機在資料值一直會有變化的時候
* 得到最後結果前，可以設置中間狀態

### `computed`
* 解決模板(View)上程式邏輯過多的問題
* 暫存運算結果
* 可以更高效解決問題

以上就是Vue的`watch`的用途與用法，`watch`雖然好用，但如果function太多，資料關聯也愈複雜的話，或許就可以考慮用`computed`寫寫看了。

-----

### 參考資料
* [Vue.js - watch](https://cn.vuejs.org/v2/api/#watch)
* [【vue】计算属性与观察者(watch/computed)](https://github.com/Kelichao/vue.js.2.0/issues/11)
* [vue中的computed和watch到底有什么不同？](https://segmentfault.com/q/1010000009263244)
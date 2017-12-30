# Day13 - [Directives] 其他指令

這篇將介紹幾個還沒介紹的指令，都有各自的用途，使用時機因人而異。

## 其他指令

以下介紹這三個指令皆**不需要表達式**。

### `v-pre`

* 用途：會在編譯過程時忽略該元素。
* 用法：

```html
<div id="app">
    <p>{{ message }}</p>
    <p v-pre>{{ message }}</p>
</div>
```

```javascript
var vm = new Vue ({
    el:'#app',
    data: {
        message: 'Hello!'
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/u2j0nu6k/1/)

### `v-cloak`

* 用途：這個指令會在編譯過程中一直保持在元素上，直到實體編譯結束，此功能可搭配css的`display`屬性，這樣就可以隱藏編譯前的內容了。
* 用法：

```html
<div id="app" v-cloak>
    {{ message }}
</div>
```

```css
[v-cloak] {
    display: none;
}
```

### `v-once`

* 用途：只會渲染元素一次，之後的重新渲染，元素或其子元素將被視為靜態內容，如果此元素不需要重複建立，可以達到**優化效能**的效果。
* 用法：

```html
<div id="app">
    <!-- 單個元素 -->
    <span v-once>{{ message }}</span>

    <!-- 含子元素 -->
    <div v-once>
        <h1>Title</h1>
        <p>{{ message }}</p>
    </div>

    <!-- 元件 -->
    <my-component v-once :comment="message"></my-component>

    <!-- 跟v-for一起使用 -->
    <ul>
        <li v-for="i in list" v-once>{{ i }}</li>
    </ul>
</div>
```

前面這幾天終於把指令通通看完與介紹完了，大致了解他們的用法後，之後實作應用上都可以使用，接下來我們將介紹一些還沒介紹到的**選項物件屬性**，這些屬性在Vue實作中也方便許多，不需要寫一堆function也可以做到想要的功能。

-----

### 參考資料
* [Vue.js API](https://vuejs.org/v2/api/index.html)
* [Vue.js (3) - 指令 (Directives) 與過濾器 (Filters)](http://blog.tonycube.com/2017/03/vuejs-3-directives-filters.html)
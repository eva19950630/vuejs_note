# Day12 - [Directives] 條件渲染(Conditional Rendering)

上一篇了解Vue的循環指令後，這篇我們將介紹也是寫程式時常用的：**條件判斷**，Vue一樣提供一些指令可以來做條件渲染(Conditional Rendering)，下面我們來詳細介紹一下。

## 條件渲染指令

### `v-show`

* 用途：根據表達式值的true或false，切換元素的`display` CSS屬性，含`v-show`的元素會根據表達式的布林值來達到顯示或消失的效果。
* 用法：

```html
<div id="app">
    <p v-show="isShow">{{ message }}</p>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        message: 'This is example of v-show',
        isShow: true
    }
});
// 每1秒改變isShow的布林值
setInterval(function() {
    vm.isShow = !vm.isShow;
}, 1000)
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/hbn0mpyt/)

### `v-if` & `v-else-if` & `v-else`

* 用途：就像是條件判斷一樣，當條件成立後，才會將某區塊資料綁定並且描繪出來。
* 用法：

###### **`v-else-if`為2.1.0版本後才加入。

#### 1. 單純使用`v-if`：

跟上面的`v-show`的例子有點像，條件判斷true會顯示，條件判斷false會消失。

```html
<div id="app">
    <p v-if="isShow">{{ message }}</p>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        message: 'This is example of v-if',
        isShow: true
    }
});
// 每2秒改變isShow的布林值
setInterval(function() {
    vm.isShow = !vm.isShow;
}, 2000)
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/uz3o9zy0/1/)

#### 2. `v-if` & `v-else-if` & `v-else`混合使用：

這就跟其他程式語言的if、else if、else的概念很像，當達到條件判斷，即會執行該條件底下的敘述(描繪畫面)。

###### **條件判斷三個等於符號(`===`)或兩個等於符號(`==`)都可以。

```html
<div id="app">
    <div v-if="type === 'A'">A</div>
    <div v-else-if="type === 'B'">B</div>
    <div v-else-if="type === 'C'">C</div>
    <div v-else>Not A/B/C</div>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        type: 'B'
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/w968so8r/)

### `v-if` vs `v-show`

雖然`v-if`跟`v-show`看起來很像，但是它們還是有一些差別的：

比較 | `v-if` | `v-show`
------------- | ------------- | -------------
多層條件判斷 | 可以加上`v-else-if`與`v-else`，所以條件判斷的狀況可以比較多。 | 只能對單獨元素做true或false的判斷。
渲染HTML元素時機 | 當條件為true才會實際渲染元素，如果一開始條件為false則不會進行任何事情。(**lazy**) | 無條件渲染元素，不管條件true或false，元素一定會被建立，然後再進行條件判斷，使用css的`display`屬性來做顯示或消失的效果。
使用`<template>`元素 | 可以使用。 | 無法使用。
在何時花費較多運算時間？ | **切換條件**時：因為當條件成立時就會重新渲染元素一次，即使元素已存在，所以在切換條件時，會花費較多運算時間。 | **初始渲染元素**時：不管條件真假，一定會將元素建立出來。
使用時機 | 條件控制比較複雜的時候，或則控制條件不太會改變，使用`v-if`比較好。 | 頻繁切換條件的時候，使用`v-show`比較好。

### `v-if` with `v-for`

`v-for`優先權高於`v-if`，也就是當這兩個指令同時出現在同一個元素時，`v-if`會隨著`v-for`的循環次數而判斷條件幾次。

範例1：

```html
<div id="app">
    <ul>
        <li v-if="n <= 5" v-for="n in 10">{{ n }}</li>
    </ul>
</div>
```

```javascript
var vm = new Vue ({
    el: '#app'
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/7yn746vo/)

範例2：

```html
<div id="app">
    <ul v-if="animalsArray.length">
        <li v-for="animal in animalsArray">{{ animal }}</li>
    </ul>
</div>
```

```javascript
var vm = new Vue ({
    el: '#app',
    data: {
        animalsArray: [ 'pig', 'dog', 'bird' ]
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/sqyqj5wh/)

前面我們知道很多directives的功能了，像是資料或屬性綁定、事件處理、列表或條件的渲染，下一篇我們再介紹幾個比較不常用，但是在某些時候使用很方便的directives。

-----

### 參考資料
* [Vue.js - v-show](https://vuejs.org/v2/api/index.html#v-show)
* [Vue.js - Conditional Rendering](https://vuejs.org/v2/guide/conditional.html)
* [Vue.js - List Rendering](https://vuejs.org/v2/guide/list.html#v-for-with-v-if)
* [Vue.js (3) - 指令 (Directives) 與過濾器 (Filters)](http://blog.tonycube.com/2017/03/vuejs-3-directives-filters.html)
* [Vue.js: 條件渲染 v-if、v-show](https://cythilya.github.io/2017/04/22/vue-conditional-rendering/)
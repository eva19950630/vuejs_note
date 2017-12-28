# Day09 - [Directives] 屬性綁定(Class and Style Binding)

前一篇介紹的指令可以將資料與vue instance之間做綁定，那假設我們想讓**HTML元素中的屬性**和vue instance做綁定，Vue有提供一個指令`v-bind`，就可以做到這樣的功能。

## 屬性綁定指令

### `v-bind`

* 用途：將HTML元素中的屬性與vue instance做綁定。
* 縮寫：`:`
* 用法：

```html
<div id="app">
    <p>讓滑鼠到連結上hover</p>
    <!-- 上下兩者結果一樣，下面為縮寫寫法 -->
    <a v-bind:href="url" v-bind:title="hint">link1</a>
    <br>
    <a :href="url" :title="hint">link2</a>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        url: 'http://www.google.com/',
        hint: '連到google網站'
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/xhfcwv4o/1/)

### 修飾符號 Modifiers

#### `.prop`

如果在`v-bind`加入`.prop`這個修飾符號，他綁定的就不是HTML元素的屬性(attribute)，而是DOM的屬性(property)。

```html
<a :href="url" :title.prop="hint">link</a>
```

#### `.camel` (2.1.0版本後才有)

若屬性名稱為kebab-case命名法，指的是有`-`分隔，可以使用`.camel`，即可將屬性名稱改成camelCase命名法(駝峰式)，但是如果一開始建立專案時選擇的樣板是有包含vue-loader或vueify的編譯器(ex. Webpack)，就不需使用`.camel`來轉換屬性名稱了。

```html
<svg :view-box.camel="viewBox"></svg>
```

#### `.sync` (2.3.0版本後才有)

這邊寫理解一個概念，如果父組件與子組件要做溝通，需使用prop語法，後面會有文章詳加介紹，而在Vue版本1.x時有`.sync`這個修飾符號，用途是當一個子組件改變一個帶有`.sync`的prop值時，父組件綁定的值也會跟著改變，也就是可以對prop做**雙向綁定**，但是因為debug比較複雜的結構時不方便，所以版本2.0後就移除`.sync`了。

但是Vue在版本2.3.0後又加回`.sync`，它現在是作為一個編譯時存在的語法糖，可以擴展成為自動更新父組件屬性的`v-on`監聽器。

###### **`v-on`在下一篇介紹事件處理(Event Handling)指令時會提到。

## 應用：Class & Style Binding

上面我們了解到`v-bind`可以將HTML元素屬性跟vue instance做綁定，在HTML元素中我們常用到`class`或`style`這兩個屬性，Vue有特別對這兩種屬性做加強功能，表達式除了可以是`string`以外，還可以使用`object`或`array`，這樣的應用稱為**Class & Style Binding**。

Vue為什麼要特別讓`v-bind`也可以接受`object`或`array`的值呢？

因為通常我們會在`class`或`style`給予比較多的值，如此一來，要綁定的值會很多，如果表達式又只有`string`，會造成vue instance不太好管理這些變數，也因此為了管理方便，Vue讓`v-bind`可以接受`object`或`array`，看看下面的範例可能會更加理解。

### 綁定多個Class屬性

#### 1. 物件`object` 寫法：

```html
<div id="app">
    <ul class="menulist">
        <li><a href="#" :class="{ active: isActive, hasError: isError }">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Work</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        isActive: true,
        isError: true
    }
});
```

#### 2. 物件`object` 寫法(集合)：

```html
<div id="app">
    <ul class="menulist">
        <li><a href="#" :class="classObject">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Work</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        classObject: {
            active: true,
            hasError: true
        }
    }
});
```

使用`computed`計算屬性來自動計算：

###### **`computed`會在後面有一篇詳細介紹，這邊先看一下寫法。

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        isActive: true,
        isError: true
    }
    computed: {
        classObject: function() {
            return {
                active: isActive,
                hasError: isError
            }
        }
    }
});
```

#### 3. 陣列`array` 寫法：

```html
<div id="app">
    <ul class="menulist">
        <li><a href="#" :class="[activeClass, errorClass]">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Work</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        activeClass: 'active',
        errorClass: 'hasError'
    }
});
```

#### 4. 加上判斷的三元運算式：

```html
<div id="app">
    <ul class="menulist">
        <li><a href="#" :class="[isActive ? activeClass : '', errorClass]">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Work</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        isActive: true,
        activeClass: 'active',
        errorClass: 'hasError'
    }
});
```

#### 5. 物件+陣列 的混合寫法：

```html
<div id="app">
    <ul class="menulist">
        <li><a href="#" :class="[{ active: isActive }, errorClass]">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Work</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        isActive: true,
        errorClass: 'hasError'
    }
});
```

上述寫法應該都會得到相同的結果，如下圖，會將HTML的class元素替換成data寫的內容。

<img src="picture/9_class bindings.png">

### 綁定多個Style屬性(inline-style)

#### 1. 物件`object` 寫法：

```html
<div id="app">
    <ul class="menulist">
        <li><a href="#" :style="{ color: anotherColor, fontSize: fontSize + 'px' }">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Work</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        anotherColor: '#ff0000',
        fontSize: '20'
    }
});
```

#### 2. 物件`object` 寫法(集合)：

```html
<div id="app">
    <ul class="menulist">
        <li><a href="#" :style="styleObject">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Work</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        styleObject: {
            color: '#ff0000',
            fontSize: '20px'
        }
    }
});
```

#### 3. 陣列`array` 寫法：

```html
<div id="app">
    <ul class="menulist">
        <li><a href="#" :style="[colorStyle, fontSizeStyle]">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Work</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        colorStyle: { 'color': '#ff0000' },
        fontSizeStyle: { 'fontSize': '20px' }
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/xgg6qnks/)

寫法跟Class大同小異，上面的寫法應該都會得到以下的結果。

<img src="picture/9_style bindings.png">

最後，從Day08與Day09可以知道只要我們設定一些指令，Vue的資料與畫面就會同步，同步的概念有以下兩種：

* 第一種：data改變，UI內容跟著更新。
* 第二種：使用者在UI輸入內容，data跟著更新。

同樣都是綁定，我們來比較一下`v-model`跟`v-bind`的差異：

* `v-model`：雙向綁定資料(第一種兼第二種)，只能在HTML表單元素和自訂元件上使用。
* `v-bind`：單向綁定(第一種)HTML元素的屬性。

介紹完資料綁定與屬性綁定後，下一篇我們將介紹**事件處理**(Event Handling)的相關指令。

-----

### 參考資料
* [Vue.js - Class and Style Bindings](https://vuejs.org/v2/guide/class-and-style.html)
* [Vue.js (7) - HTML 的 Class 與 Style 屬性綁定](http://blog.tonycube.com/2017/04/vuejs-7-html-class-style.html)
* [Vue.js: 屬性綁定 v-bind、Class 與 Style 綁定](https://cythilya.github.io/2017/04/21/vue-v-bind-class-and-style/)
* [Styling with Inline CSS Styles in Vue.js](https://codingexplained.com/coding/front-end/vue-js/styling-inline-css-styles)
# Day08 - [Directives] 資料綁定(Data Binding)

## 推薦好用的工具：Vue.js devtools

這篇介紹開始之前，最近發現一個開發vue時還不錯用的chrome插件，想推薦大家使用，這個插件是[Vue.js devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)，它可以列出components還有vue instance的屬性，方便開發及debug，當你安裝之後，在chrome的開發者工具上面的tab會多一個Vue tab，執行vue環境並點選，點下去即可看見以下的畫面。

<img src="picture/8_vue devtools.png">

回到正題

從這一天開始，我們會連續幾天介紹Vue.js所提供一系列`v-`開頭的**指令**(Directives)，指令的用途是讓網頁的DOM元素直接去讀取或存入vue instance的data，我們可以將指令寫在HTML語法裡面，並給予指令適當的表達式(有些指令不需要表達式)，如此一來，指令就會將結果呈現在DOM上。

我們依照指令用途來做分類，首先這篇會先介紹**資料綁定**(Data Binding)的相關指令及用法。

## 為什麼要做資料綁定？

我們在創建vue instance與components時，通常會各自丟data在裡面，然後前端會寫一些html語法來接收這些data，或者使用者會在browser輸入一些資訊，如果這之間沒有做資料綁定，使用者就無法在前端browser中看到你想要呈現的資訊，也就無法進一步再操作，因此Vue提供一些指令可以來做資料綁定。

那在了解資料綁定指令有哪些之前，前面文章我們提到過創建vue instance時可以傳入**選項物件**(Options)，物件中可以寫入一些屬性，這邊我們先花一些篇幅認識一個常用的屬性，`data`(資料)。

## 選項物件屬性：`data`

`data`可用來儲存元件內部狀態或資料，其資料型態可以是`object`或`function`，但需要注意的是，各元件檔(.vue)為各自獨立非共用，所以元件中的data只能是`function`型態。範例如下：

```html
<div id="app">
    {{ message }}
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        message: 'Hello Test!'
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/4pL6fydu/)

元件中的data只能是`function`型態：
```javascript
export default {
    data () {
        return {
            message: 'Hello Test!'
        }
    }
}
```

了解data屬性的操作後，以下我們來介紹vue常用的資料綁定指令。

## 資料綁定指令

### `v-text`

* 用途：更新被指定元素的`textContent`，也就是該元素的整個內容都會被更新，如果想要更新部分內容，就需要使用雙大括號`{{ }}`。
* 接受值：`string`
* 用法：

```html
<div id="app">
    <span v-text="msg"></span>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        msg: 'Hello Test!'
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/ep4sf7n3/)

如果只要更新**部分**內容，可以使用雙大括號`{{ }}`。
```html
<div id="app">
    <span>{{ msg }}</span>
</div>
```

以上兩個範例的結果會是一樣的，通常我們會使用雙大括號的方法，因為他除了更改部分內容外，也比較有使用彈性。

### `v-html`

* 用途：更新被指定元素的`innerHTML`，內容以普通HTML語法插入，不會作為Vue模板來進行編譯。
* 接受值：`string`
* 用法：

```html
<div v-html="html"></div>
```

注意不要使用此指令任意接受其他不可信任的HTML，很容易會導致有[XSS攻擊](https://zh.wikipedia.org/wiki/%E8%B7%A8%E7%B6%B2%E7%AB%99%E6%8C%87%E4%BB%A4%E7%A2%BC)的風險。

若單一元件(.vue)中css style有寫`scoped`，`v-html`的html內容不會套用有寫`scoped`的css style，因為`v-html`的html內容並沒有被Vue模板編譯器編譯過。

在元件檔裡面的style寫`scoped`的用途是讓這style標籤內的所有樣式只會套用在**這個元件檔**裡面的所有元素。寫法如下：
```html
<style scoped>
    /*write down your css here*/
</style>
```

### `v-model`

`v-model`是一個常用的指令，通常會用來做表單資料的**雙向綁定**(Two-way Binding)，意思就是說將View與資料綁在一起，當使用者輸入資料到輸入框後，會自動將資料存在一個變數中，並即時更新資料到綁定的View當中，下面我們介紹`v-model`的用途及用法。

* 用途：對表單元素做雙向綁定，並即時將輸入資料更新到綁定的View中。
* 用法：只能在**表單元素**或**自訂Vue元件**上使用。

#### 1. 單行輸入框 `input text`

```html
<div id="app">
    <input type="text" v-model="message">
    <div>{{ message }}</div>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        message: 'Hello Test!'
    }
});
```

###### **可以試試看在結果那邊的input框內新增或刪除文字，看看會有什麼變化。

* [run on JSFiddle](https://jsfiddle.net/eva19950630/2282194o/)

#### 2. 多行輸入框 `textarea`

```html
<div id="app">
    <textarea v-model="message"></textarea>
    <div>{{ message }}</div>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        message: 'Hello Test!'
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/LwL0f98m/)

#### 3. 單選按鈕 `radio`

```html
<div id="app">
    <input type="radio" v-model="selected" value="Item1"><label>Item1</label>
    <input type="radio" v-model="selected" value="Item2"><label>Item2</label>
    <input type="radio" v-model="selected" value="Item3"><label>Item3</label>
    <div>selected = {{ selected }}</div>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        selected: 'Please choose any items.'
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/4vk8r5dc/)

#### 4. 複選按鈕 `checkbox`

因為是複選，所以資料屬性不能是字串，要改成陣列。

```html
<div id="app">
    <input type="checkbox" v-model="selected_group" value="Item1"><label>Item1</label>
    <input type="checkbox" v-model="selected_group" value="Item2"><label>Item2</label>
    <input type="checkbox" v-model="selected_group" value="Item3"><label>Item3</label>
    <div>selected_group = {{ selected_group }}</div>
</div>
```

```javascript
// 宣告data中的變數為陣列[]
var vm = new Vue({
    el: '#app',
    data: {
        selected_group: []
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/vfdfrfxk/)

#### 5. 下拉式選單 `select`

當其中一個`option`被選擇，該`option`的值(value)或內容則會被存在`selected`這個資料屬性中，如果該`option`沒有設定`value`屬性，則只會存其內容。

```html
<div id="app">
    <select v-model="selected">
        <option>Item1</option>
        <option>Item2</option>
        <option value="Item3_value">Item3</option>
    </select>
    <div>selected = {{ selected }}</div>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        selected: null
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/9qn9kc4g/)

### 修飾符號 Modifiers

#### `.lazy`

當`v-model`與`input`事件綁定時，使用者輸入的內容會同步更新輸入框的值和資料，但是如果加上`.lazy`，會改成`onChange`事件監聽，讓輸入框失去焦點，也就是說，當我們輸入內容進`input`時，輸出資料不會立即反應，要將滑鼠點至`input`框外，才會看到結果。

```html
<input type="text" v-model.lazy="message">
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/62u46z6k/)

#### `.number`

`v-model`預設得到的值，型態為`string`，而`.number`可以做到的是把字串強制轉為數字。

```html
<input type="text" v-model.number="count">
```

###### **在結果那邊如果輸入數字1,2,3，型態應該是`number`，如果將html語法裡面的`.number`刪除再執行，並輸入數字，看看會有什麼變化。

* [run on JSFiddle](https://jsfiddle.net/eva19950630/1mLg0rp7/) 

#### `.trim`

`.trim`可用來去除input框內容中的首尾空格，如此一來，就不需要使用js中的trim()函式處理這部分了。它是`onChange`事件監聽，所以不會立即同步，將滑鼠點至input框外，才會看到結果。

```html
<input type="text" v-model.trim="message">
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/qff158su/)


上面介紹Options的data屬性以及Vue資料綁定的指令與用法，下一篇我們即將介紹**屬性綁定**的相關指令。

-----

### 參考資料
* [wiki](https://zh.wikipedia.org/wiki/)
* [Vue.js (3) - 指令 (Directives) 與過濾器 (Filters)](http://blog.tonycube.com/2017/03/vuejs-3-directives-filters.html)
* [Vue.js (5) - 表單資料雙向綁定指令](http://blog.tonycube.com/2017/04/vuejs-5-form.html)
* [Vue.js: data、v-model 與雙向綁定](https://cythilya.github.io/2017/04/14/vue-data-v-model/)
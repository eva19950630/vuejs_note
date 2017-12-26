# Day08 - [Directives] 資料綁定(Data Binding)

從這一天開始，我們會連續幾天介紹Vue.js所提供一系列`v-`開頭的**指令**(Directives)，除了`v-pre`、`v-cloak`、`v-once`，我們可以在其他指令的表達式中給予**值**，指令就會將結果呈現在DOM上。

我們依照指令用途來做分類，首先這篇會先介紹**資料綁定**(Data Binding)的相關指令及用法。

## 為什麼要做資料綁定？

我們在創建**Vue Instance**與**Components**時，通常會各自丟`data`在裡面，但是如果這之間沒有做資料綁定，使用者就無法在前端browser中看到你想要呈現的資訊，也就無法進一步再操作。

因此Vue提供一些指令可以來做資料綁定，以下我們就來做詳細介紹。

## 資料綁定指令

### `v-text`

* 接受值：`string`
* 用途：更新被指定元素的`textContent`，也就是該元素的整個內容都會被更新，如果想要更新部分內容，就需要使用雙大括號`{{ }}`。
* 用法：有兩種方法。

1. 更新**全部**內容
```html
<div id="#app">
    <span v-text="msg"></span>
</div>
```
2. 更新**部分**內容 
```html
<div id="#app">
    <span>{{ msg }}</span>
</div>
```

以上兩個範例的結果會是一樣的，通常我們會使用雙大括號的方法，因為他除了更改部分內容外，也比較有使用彈性。

### `v-html`

* 接受值：`string`
* 用途：更新被指定元素的`innerHTML`，內容以普通HTML語法插入，不會作為Vue模板來進行編譯。
* 用法：

```html
<div v-html="html"></div>
```

注意不要使用此指令任意接受其他不可信任的HTML，很容易會導致有[XSS攻擊](https://zh.wikipedia.org/wiki/%E8%B7%A8%E7%B6%B2%E7%AB%99%E6%8C%87%E4%BB%A4%E7%A2%BC)的風險。

若單一元件**.vue檔**中css style有寫`scoped`，`v-html`的html內容不會套用有寫`scoped`的css style，因為`v-html`的html內容並沒有被Vue模板編譯器編譯過。

在元件檔裡面的style寫`scoped`的用途是讓這style標籤內的所有樣式只會套用在**這個元件檔**裡面的所有元素。寫法如下：
```html
<style scoped>
    /*write down your css here*/
</style>
```

### `v-model`

`v-model`是一個常用的指令，通常會用來做表單資料的**雙向綁定**(Two-way Binding)，意思就是說將View與資料綁在一起，當使用者輸入資料到輸入框後，會自動將資料存在一個變數中，並即時更新資料到綁定的View當中。

前面文章我們提到過vue instance可以傳入**選項物件**(Options)，這邊我們先花一些篇幅認識一個常使用的屬性，`data`(資料)。

#### 選項物件屬性：`data`

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

回來看`v-model`，下面我們介紹`v-model`的用途及用法。

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

* [run on JSFiddle](https://jsfiddle.net/eva19950630/2282194o/)

###### **可以試試看在結果那邊的input框內新增或刪除文字，看看會有什麼變化。

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

#### 6. 修飾符號 Modifiers

##### `.lazy`

當`v-model`與`input`事件綁定時，使用者輸入的內容會同步更新輸入框的值和資料，但是如果加上`.lazy`，會改成`change`事件監聽，會讓輸入框失去焦點，也就是說，當我們輸入內容進`input`時，輸出資料不會立即反應，要將滑鼠點至`input`框外，才會看到結果。

```html
<input type="text" v-model.lazy="message">
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/62u46z6k/)

##### `.number`

`v-model`預設得到的值，型態為`string`，而`.number`可以做到的是把字串強制轉為數字。

```html
<input type="text" v-model.number="count">
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/1mLg0rp7/) 

###### **在結果那邊如果輸入數字1,2,3，型態應該是`number`，如果將html語法裡面的`.number`刪除再執行，並輸入數字，看看會有什麼變化。

##### `.trim`

`.trim`可用來去除input框內容中的首尾空格，如此一來，就不需要使用js中的trim()函式處理這部分了。它是`change`事件監聽，所以不會立即同步，將滑鼠點至input框外，才會看到結果。

```html
<input type="text" v-model.trim="message">
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/qff158su/)


上面介紹Options的data屬性以及Vue資料綁定的指令與用法，下一篇我們即將介紹**事件處理**(Event Handling)的相關指令。

-----

### 參考資料
* [wiki](https://zh.wikipedia.org/wiki/)
* [Vue.js (3) - 指令 (Directives) 與過濾器 (Filters)](http://blog.tonycube.com/2017/03/vuejs-3-directives-filters.html)
* [Vue.js (5) - 表單資料雙向綁定指令](http://blog.tonycube.com/2017/04/vuejs-5-form.html)
* [Vue.js: data、v-model 與雙向綁定](https://cythilya.github.io/2017/04/14/vue-data-v-model/)
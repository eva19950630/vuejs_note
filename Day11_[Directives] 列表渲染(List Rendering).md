# Day11 - [Directives] 列表渲染(List Rendering)

寫程式的我們都知道，如果想要把陣列或物件裡所有東西print出來，就需要用到**迴圈**來寫，在Vue中，也有可以放在HTML元素中的循環指令，把陣列或物件的所有元素顯示出來，官方稱這個叫做**列表渲染**(List Rendering)，下面我們就來看看Vue的指令要如何做到跟迴圈一樣的效果。

## 列表渲染指令

### `v-for`

* 用途：重複迭代顯示陣列或物件中的元素，類似loop的概念。
* 表達式：`array`、`object`、`number`、`string`
* 用法：`alias in expression` (`別名 in 表達式`)

#### 1. 陣列循環`array` 寫法：

以下範例說明：data那邊宣告一個memberArray陣列`[]`，member是自定義的別名，會暫存從memberArray陣列取出的元素，進而可以使用該屬性，`member.id`與`member.name`，`index`代表索引值。

```html
<div id="app">
    <ul>
        <li v-for="(member, index) in memberArray">
            index: {{ index }} ,
            id: {{ member.id }} ,
            name: {{ member.name }}
        </li>
    </ul>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        memberArray: [
            { id: '11', name:'Eva' },
            { id: '12', name:'Ray' },
            { id: '13', name:'Ben' }
        ]
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/f7jc2qy3/1/)

#### 2. 物件循環`object` 寫法：

```html
<div id="app">
    <ul>
        <li v-for="member in memberObject">
            {{ member }}
        </li>
    </ul>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        memberObject: {
            name: 'Eva',
            gender: 'female',
            age: '22'
        }
    }
});
```

從上面範例可以看出，物件是以`key: value`儲存資料，Vue提供第二個參數取得key。

```html
<ul>
    <li v-for="(member, key) in memberObject">
        {{ key }} : {{ member }}
    </li>
</ul>
```

可以再加入第三個參數`index`，作為索引值。

```html
<ul>
    <li v-for="(member, key, index) in memberObject">
        {{ index }} . {{ key }} : {{ member }}
    </li>
</ul>
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/m1rx76xn/1/)

#### 3. 整數循環`number` 寫法：

```html
<div id="app">
    <span v-for="n in 5">{{ n }}</span>
</div>
```

```javascript
var vm = new Vue({
    el: '#app'
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/swmmg6mv/)

#### 4. 字串循環`string` 寫法：

```html
<div id="app">
    <span v-for="string in 'Vue'">{{ string }}-</span>
</div>
```

```javascript
var vm = new Vue({
    el: '#app'
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/bmc7grm9/)

### key

`v-for`已經為列表渲染提供很高的效能了，但是假設我們要更新陣列中的元素呢？

Vue盡量將列表資料渲染出來，但是在沒設定key值的情況下，當我們改變陣列中的元素，會造成部分元素更新，而不會重新渲染元素。

另外前面我們提到過Virtual DOM會使用DOM diff演算法來比較這一次與上一次Virtual DOM的差異，所以當列表中有資料異動時，Virtual DOM是會這樣操作的：

<img src="picture/11_virtual dom diff.png">

`DOM diff`比較出差異後，將C更新成F、D更新成C、E更新成D、最後再放入E，如此一來，效率就沒有比較好。

也因此為了辨識每一項元素，Vue提供key可以為每一項元素設定key值，看看下圖比較一下沒有設定key值與有設定key值的差異：

<img src="picture/11_without key vs key.png">

所以設定key值的目的就是在Virtual DOM更新的時候，提供較高的效能，並使用`v-bind`綁定key屬性`:key`，讓陣列或物件裡的元素保有唯一性，當元素更新時，可以確保重新渲染元素。

```html
<div v-for="item in items" :key="item.id">
    <!-- content -->
</div>
```

### 陣列元素操作

Vue提供一組可以操作陣列的method，用來變更陣列元素，觀察其變更。

method | 用途
------------- | -------------
push() | 新增元素。
pop() | 刪除最後加入的元素。
shift() | 刪除第一個元素。
unshift() | 加入元素至第一個位置。
splice() | 加入或移除元素。
sort() | 由小至大排序。
reverse() | 元素排序排列。
filter() | 過濾陣列的元素，將符合條件的傳回並形成一個新陣列。
concat() | 連接陣列，傳回一個新陣列。
slice() | 切割陣列，傳回一個新陣列。

其實Vue的**列表渲染**可以做很多事情，像是使用`computed`來過濾或排序陣列元素，或則應用在做一個todo list、分頁功能等等，後面在實作應用上會舉例給大家看。

-----

### 參考資料
* [Vue - v-for](https://vuejs.org/v2/api/index.html#v-for)
* [Vue - List Rendering](https://vuejs.org/v2/guide/list.html)
* [Vue.js: 列表渲染 v-for](https://cythilya.github.io/2017/04/27/vue-list-rendering/)
* [Vue2.0 v-for 中 :key 到底有什么用？](https://www.zhihu.com/question/61064119)
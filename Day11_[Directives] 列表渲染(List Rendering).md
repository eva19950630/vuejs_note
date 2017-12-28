# Day11 - [Directives] 列表渲染(List Rendering)

寫程式的我們都知道，如果想要把陣列或物件裡所有東西print出來，就需要用到**迴圈**來寫，在Vue中，也有可以放在HTML元素中的循環指令，把陣列或物件的所有元素顯示出來，官方稱這個叫做列表渲染(List Rendering)，下面我們就來看看Vue的指令要如何做到跟迴圈一樣的效果。

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

* [run on JSFiddle](https://jsfiddle.net/eva19950630/m1rx76xn/)

key

#### 3. 整數循環`number` 寫法：



#### 4. 字串循環`string` 寫法：



key


陣列元素操作


使用`computed`做過濾或排序


-----

### 參考資料


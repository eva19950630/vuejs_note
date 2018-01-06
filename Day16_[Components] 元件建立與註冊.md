# Day16 - [Components] 元件建立與註冊

前面這幾天已經把Vue.js的基本功能大概都介紹完了，但別忘了Vue.js最強大的功能之一：**元件**(component)，讓我們恢復一下記憶，當vue constructor建立好vue instance後，我們可以將寫好的元件放入，建構好樹狀結構，每個元件就像是一塊塊拼圖一樣，擁有自己的邊邊角角，這些都是有用處的，也就是說元件會封裝好需要的功能，接著拼一拼就能拼出一張完整的拼圖，如果元件之間沒有做組合(Day17會介紹)，每個元件其實互相干擾的程度很小，各自獨立運作封裝好的功能，因此對開發者來說是很容易維護的。 

接下來這幾天我們將介紹元件要如何建立與組合，以及一些特別的功能，那首先元件要如何開始使用呢，以下詳細說明。

## 元件建立與註冊

Vue.js提倡輕量，如果能夠縮減或讓模板簡潔的方法，幾乎都有相對應功能的語法可以去套用，而元件就像是vue instance的縮減版，先來看看之前我們是怎麼宣告vue instance(將之前學過的options屬性一併列出)：

```javascript
var vm = new Vue ({
    // options
    el: '#app',
    data: { ... },
    methods: { ... },
    filters: { ... },
    computed: { ... },
    watch: { ... }
})
```

而元件可以分成兩種：**全域元件**與**區域元件**，以下為建立全域元件或區域元件的方式。

### 全域元件(Global Component)

利用`Vue.component()`方法直接建立的為全域元件。

#### 全域註冊(Global Registration)

需使用`Vue.component(tagName, options)`來全域註冊元件，必須在vue instance初始化之前建立完成。

* tagName：元件的tag名稱，可以在HTML中被拿來當標籤使用，所以須保有唯一性，不能與其他全域元件的名稱重複。
* options：選擇性參數，可將前面所學到的options屬性(`data`、`methods`、`filters`、`computed`、`watch`)放進去使用，注意`data`只能是`function`型態，如果沒有使用`function`，Vue將會停止執行。

> 在元件中`data`只能是`function`型態的原因：
因為元件都是獨立的，為了不互相干擾，所以每次回傳資料都會新建立一次資料，而不是同一個資料，否則當改變其中一個元件資料時，另一個元件資料也會跟著動到。

```javascript
Vue.component('tagName', {
    // options
})
```

範例如下：

```html
<div id="app">
    <global-component></global-component>
</div>
```

```javascript
Vue.component('global-component', {
    template: '<div><p>{{ message }}</p><button @click="notice">點我</button></div>',
    data: function() {
        return {
            message: '這是全域註冊的元件'
        }
    },
    methods: {
        notice: function() {
            alert('全域註冊的元件裡面也可以寫methods!');
        }
    }
})

var vm = new Vue ({
    el: '#app'
})
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/ww0zvcht/1/)

### 區域元件(Local Component)

放在vue instance內，變成vue instance的其中一個options屬性，只能供該實體使用的為區域元件。

#### 局部註冊(Local Registration)

將component寫在vue instance裡面。

範例如下：

```html
<div id="app">
    <local-component></local-component>
</div>
```

```javascript
// declare a new variable and put options what this component needs
var local_component = {
    template: '<div><p>{{ message }}</p><button @click="notice">點我</button></div>',
    data: function() {
        return {
            message: '這是局部註冊的元件'
        }
    },
    methods: {
        notice: function() {
            alert('Local Component!');
        }
    }
}

var vm = new Vue ({
    el: '#app',
    components: {
        'local-component': local_component
    }
})
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/zjw3Ldwe/1/)

了解完怎麼建立元件之後，下一篇我們將學習怎麼將讓元件之間組合並且溝通。

-----

### 參考資料
* [Vue.js - Components](https://vuejs.org/v2/guide/components.html)
* [Vue.js (9.1) - 元件(Component)](http://blog.tonycube.com/2017/05/vuejs-9-1-component.html)
* [Vue.js: 元件 Components 簡介 - 註冊與使用](https://cythilya.github.io/2017/05/11/vue-component-intro/)
* [Vue.js 13 - 組件/元件(Component) - 用途及使用方式](https://ithelp.ithome.com.tw/articles/10187877)
# Day17 - [Components] 元件組合與溝通

雖然每個元件是獨立運作，但Vue設計元件的目的是為了讓每個元件都有各自的用途，然後再互相配合使用，如此一來，系統開發上也比較結構化。

比較常見的組合元件為**父子元件**，當一個A元件包含另一個B元件，則A、B元件就形成了父子關係(parent-child relationship)，那要如何讓他們之間做溝通呢？以下我們來詳細介紹。

## 父子元件溝通(`Props down, Events up`)

<img src="picture/17_props-events.png">

從上圖來看，我們可以大概看出一個溝通模式：`Props down, Events up`，父元件透過`props`向下傳遞資料給子元件，而子元件則是透過`events`將結果向上傳回給父元件。

### 父元件對子元件的`Props down`

因為元件的作用範圍是獨立的，所以當子元件想接收父元件的資料時，我們是不能直接去引用父元件的資料的，因此父元件想要傳遞資料給子元件時，可以使用`props`屬性，將資料傳遞給子元件。

範例：

```html
<div id="app">
    <child :name='data_name' message='I am a child.'></child>
</div>
```

```javascript
Vue.component('child', {
    props: ['name', 'message'],
    template: `<p>
                <font color=red>{{ name }}</font> says
                <font color=red>{{ message }}</font>
               </p>`
})

var vm = new Vue ({
    el: '#app',
    data: {
        data_name: 'Mary'
    }
})
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/3adm6uqu/1/)

> 從上述的範例來看，我們建立一個全域元件`child`，並在模板使用此元件時定義了兩個屬性`name`、`message`，其中`name`有使用`v-bind`指令：
> * 自製元件`child`，裡面有一個自訂屬性`props`，它會去接收寫在模板的屬性`name`、`message`下的資料。
> * 使用`v-bind`指令是為了帶入vue instance `data`中的`data_name`。

我們整理一下`props`的功能：
* 為元件中的自訂屬性。
* 它的接受值可以是**陣列**或**物件**。
* 用來接收父元件的資料。

#### camelCase vs kebab-case

在HTML中，屬性是不區分大小寫的，也就是說如果你在HTML中寫`myattribute`跟寫`myAttribute`，HTML會看成是一樣的東西，所以如果我們在元件中使用`props`去接收屬性的資料時，寫的是camelCase(駝峰式命名法)的屬性名稱，在HTML中就要自動轉換為kebab-case(用dash間隔的命名法)。

註：如果你使用的是**字串模板**，則不會有這個命名的問題存在。

範例：

```html
<div id="app">
    <child my-message="HelloWorld"></child>
</div>
```

```javascript
Vue.component('child', {
    props: ['myMessage'],
    template: '<p>{{ myMessage }}</p>'
})

var vm = new Vue ({
    el: '#app'
})
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/gxakwazL/)

#### 動態Props(Dynamic Props)

將`props`接收資料的方法結合屬性綁定的指令`v-bind`和資料雙向綁定的指令`v-model`，當父元件的資料改變時，子元件資料也會跟著改變，來達到動態資料變化的效果。

範例：試著改變input裡面的字，可以發現原本顯示的字也會跟著改變。

```html
<div id="app">
    <input type="text" v-model="message">
    <child :my-message="message"></child>
</div>
```

```javascript
Vue.component('child', {
    props: ['myMessage'],
    template: '<p>{{ myMessage }}</p>'
})

var vm = new Vue ({
    el: '#app',
    data: {
        message: 'HelloWorld'
    }
})
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/ff3b6xn1/)

#### 使用`props`的小細節：傳入的資料是字串還是數值？

如果我們只是透過`props`來接收模板屬性資料時，因為模板不會做任何處理，所以我們收到的資料型態為`string`，但是我們假設我們想收到的資料型態為`number`，則我們必須使用`v-bind`指令，在模板編譯的時候會被當成JavaScript表達式來做計算。

範例：如果屬性沒有使用`v-bind`指令綁定，我們收到的”15“就是`string`，“+1”後會變成"151"。

```html
<div id="app">
    <p>沒有使用v-bind：<child price="15"></child></p>
    <p>有使用v-bind：<child :price="15"></child></p>
</div>
```

```javascript
Vue.component('child', {
    props: ['price'],
    template: '<span>{{ price + 1 }} ({{ typeof price }})</span>'
})

var vm = new Vue ({
    el: '#app'
})
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/7a5n8wa0/)

#### 單向資料流(One-Way Data Flow)

Prop是單向綁定的，也就是當父元件屬性資料改變時，只能**單向傳遞資料給子元件**，反過來是不行的，目的是為了不讓子元件可以任意去更改父元件的狀態。

注意，當父元件更新時，子元件的所有prop也會跟著更新為最新的資料。

但是如果我們還是想要從子元件中更動父元件的資料狀態時，我們可以使用區域變數(local variable)或`computed`的方式來做。

範例：如果我們直接在元件的`data`中，將`this.name`指派別的值，Vue會在編譯時發出警告。

```html
<div id="app">
    <child name="Mary"></child>
</div>
```

```javascript
Vue.component('child', {
    props: ['name'],
    data: function() {
        this.name = this.name + "is a girl.";
        return {
            myName: this.name;
        }
    },
    template: '<p>{{ myName }}</p>'
})

var vm = new Vue ({
    el: '#app'
})
```

改寫1：使用區域變數

```javascript
Vue.component('child', {
    props: ['name'],
    data: function() {
        var myName = '';
        myName = this.name + "is a girl.";
        return {
            myName: myName;
        }
    },
    template: '<p>{{ myName }}</p>'
})
```

改寫2：使用`computed`

```javascript
Vue.component('child', {
    props: ['name'],
    computed: {
        myName: function() {
            return this.name + "is a girl.";
        }
    }
    template: '<p>{{ myName }}</p>'
})
```

#### Prop驗證(Prop Validation)

我們可以在元件定義Prop的資料型態，當傳入的資料不符合該型態時，Vue就會提出警告。

通常會使用物件的方式來定義資料型態，但是如果有多個資料型態，可以使用陣列。

```javascript
Vue.component('child', {
    props: {
        // 直接定義屬性
        id: Number,

        // 使用陣列來定義多個型態
        name: [String, Number],

        // 使用物件來寫一些規則
        age: {
            type: String,
            default: '',
            validator: function(value) {
                return value > 0;
            },
            required: true
        }
    }
})
```

物件中的規則說明如下：
* `type`：資料型態
* `default`：預設值
* `validator`：檢驗屬性資料是否有效
* `required`：該屬性是否為必要

`type`可以有的屬性如下：
* `String`
* `Number`
* `Boolean`
* `Function`
* `Object`
* `Array`
* `Symbol`

`type`可以是自訂的建構式函式，使用`instanceof`來檢測。

注意，因為元件會在vue instance創建之前建立好，所以在`default`或`validator`函數裡，不能使用`data`、`computed`、`methods`等屬性無法使用。

### 子元件對父元件的`Events up`

我們學會父元件傳遞資料給子元件，那子元件要怎麼回應給父元件呢？

子元件回應的方式是`Events up`，就是Vue的事件處理，使用`v-on`接收事件觸發並且去呼叫對應函式，對`v-on`的詳細介紹可以去看[Day10](https://ithelp.ithome.com.tw/articles/10194631)的內容，下面我們就來介紹子元件要怎麼使用`events`去回應父元件。

#### 一個Vue Instance的`事件介面`(Event Interface)實作方式包含以下兩種：

* 使用`$on(eventName)`監聽事件
* 使用`$emit(eventName)`觸發事件

在HTML模板中，子元件即使用`v-on`綁定事件。

#### 元件綁定原生事件

如果我們想在某個元件上監聽一個原生事件，可以使用`v-on`的修飾符`.native`來寫：

```html
<my-component @click.native="doSomething"></my-component>
```

## 非父子元件溝通(global event bus)

如果是兩個子元件要互相溝通，因為元件都是獨立運作在自己作用域的，因此我們可以透過創建一個空的Vue Instance作為事件的總管理線。

```javascript
// 創建空的Vue Instance "bus"
var bus = new Vue()

// 觸發A元件中的事件 "id-selected"
bus.$emit('id-selected', 1)

// 在B元件的 "created" 鉤子中監聽 "id-selected" 這個事件
bus.$on('id-selected', function(id) {
    // content
})
```

但這只能適用於**小型網站**應用開發，試想如果有很多子元件都透過這樣的方式溝通，會讓這個`bus`管理線變得很繁忙複雜，因此中大型網站官方推薦使用**Vuex**，它是一個狀態管理模式，後面Day20我們會提到關於Vuex的概念與應用。

-----

### 參考資料
* [Vue.js - Composing Components](https://vuejs.org/v2/guide/components.html#Composing-Components)
* [Vue.js (9.1) - 元件(Component)](http://blog.tonycube.com/2017/05/vuejs-9-1-component.html)
* [Vue.js (9.2) - 元件(Component)](http://blog.tonycube.com/2017/05/vuejs-9-2-component.html)
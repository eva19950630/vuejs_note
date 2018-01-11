# Day18 - [Components] 插槽(Slot)

Vue的元件化系統，讓開發者在開發上可以很好維護，主要是因為元件都有各自的用途與模板，而上一篇也介紹過元件之間要如何去做組合溝通，那如果一個網站要放入很多的元件，先來看看下方官網提供的例子：

```html
<app>
    <app-header></app-header>
    <app-footer></app-footer>
</app>
```

這樣的組合可能會兩個問題發生：
* `<app>`元件內容是由父元件提供，所以可能會不知道收到的資料是什麼。
* `<app>`元件其實有自己的模板，這樣合併會造成顯示出來的不是自己想要的結果。

我們其實很容易會去使用元件中塞入元件的功能，因為元件只能在自己掛載的指定範圍渲染畫面或做事情，舉例來說，像是網頁上面的navbar導覽列，每個頁面其實都會放navbar，可能只是有一些些功能的不同，這時候就需要使用一個Vue提供的特殊元素：`slot`。

### `slot`使用方法

先來看看如果沒有使用`slot`的範例：

```html
<div id="app">
    <child>NO SLOT: {{ msg }}</child>
    <p>{{ msg }}</p>
</div>
```

```javascript
Vue.component('child', {
    template: '<div><p>{{ msg }}</p></div>',
    data: function() {
        return {
            msg: 'Component Message'
        }
    }
})

var vm = new Vue ({
    el: '#app',
    data: {
        msg: 'Vue instance Message'
    }
})
```

* [run on JSFiddle][1]

我們會發現在HTML裡面`<child></child>`裡面包的內容沒有顯示，主要是因為它們被我們自定義好的子元件下的`template`給取代掉了，但是當我們有用`slot`，這種情況就可以避免。

將上面的範例加入`slot`：只要將`<slot></slot>`放進子元件下的`template`內就可以了。

```html
<div id="app">
    <child>HAVE SLOT: {{ msg }}</child>
    <p>{{ msg }}</p>
</div>
```

```javascript
Vue.component('child', {
    template: '<div><slot></slot><p>{{ msg }}</p></div>',
    data: function() {
        return {
            msg: 'Component Message'
        }
    }
})

var vm = new Vue ({
    el: '#app',
    data: {
        msg: 'Vue instance Message'
    }
})
```

* [run on JSFiddle][2]

我們加入`<slot></slot>`後，發現原本寫在HTML的`<child>`裡面的內容就會出現了，`{{ msg }}`收到的資料為我們定義在vue instance裡面的data資料。

總結一下`slot`的功能，`<slot></slot>`就是一個插槽，這個插槽就像是一個容器一樣，它可以放入**父元件**所指定的資料。

### `slot`種類目前可分為三種

#### 1. 單個插槽(Single Slot)

我們將`<slot></slot>`寫在子元件裡，`<slot></slot>`會去接收父元件所指定的資料，如果沒有指令資料，則會顯示原本寫在子元件中`<slot></slot>`內的預設資料。

範例：

```html
<div id="app">
    <p>當父元件有指定資料時：<child>HELLO WORLD</child></p>
    <p>當父元件沒有指定資料時：<child></child></p>
</div>
```

```javascript
Vue.component('child', {
    template: '<span><slot>This is a default single slot.</slot></span>'
})

new Vue ({
    el: '#app'
})
```

* [run on JSFiddle][3]

#### 2. 具名插槽(Named Slots)

我們也可以為`slot`定義名稱，這樣會比較好分辨，如此一來，父元件就可以指令要在哪一個`slot`放入哪個指定資料。

使用方式是加入一個屬性`name`，`<slot name="slot-name"></slot>`。

範例：用**Named Slots**就很適合做網頁的navbar與footer。

`html
<div id="app">
    <child>
        <p slot="header">Header</p>
        <p>Main Content</p>
        <p slot="footer">Footer</p>
    </child>
</div>
`

```javascript
Vue.component('child', {
    template: `<div>
                  <header>
                    <slot name="header"></slot>
                  </header>
                  <main>
                    <slot></slot>
                  </main>
                  <footer>
                    <slot name="footer"></slot>
                  </footer>
                </div>`
})

new Vue ({
    el: '#app'
})
```

* [run on JSFiddle][4]

> 註：如果你要在`template`下放入多行程式碼(會分好幾行的意思)，使用「 ' 」會錯誤，可以使用**「 \` 」**。

#### 3. 作用域插槽(Scoped Slots)

這是2.1.0版本後才新增的特殊類型插槽。如果我們在父元件的元素與`slot`做一些設定，`slot`就可以將資料傳遞到父元件，跟上一篇使用過的`props`傳遞資料的用法很像。

範例：資料傳遞與接收

我們可以將要傳遞的資料放入子元件`slot`下的`text`，然後父元件那邊用`scope`代表這是給Scoped Slots使用的，`props`用來保存從子元件傳遞過來的資料。

```html
<div id="app">
    <child>
        <template scope="props">
            <p>{{ props.text }}</p>
        </template>
    </child>
</div>
```

```javascript
Vue.component('child', {
        template: `<div>
                        <slot text="This is a text from slot."></slot>  
                    <div>`
})

new Vue ({
    el: '#app'
})
```

> 目前版本2.5.0以上，`scope`已經可以放在其他元素下，不限定要放在`<template>`裡面了。但因為JSFiddle的版本為v2.2.1，所以範例程式還是寫在`<template>`之下。

* [run on JSFiddle][5]

總結一下，`slot`是元件中一個好用的特殊功能，尤其Vue非常注重元件化，有了`slot`，我們可以在開發網頁上比較有架構，維護上也更方便。

---- 

### 參考資料
* [Vue.js - Content Distribution with Slots][6]
* [Vue.js: Slot][7]
* [Vue.js (9.2) - 元件(Component)][8]
* [vue & vuex 09 - component - III (slot 在元件上鑽洞)][9]

[1]:	https://jsfiddle.net/eva19950630/gqmtg42p/1/
[2]:	https://jsfiddle.net/eva19950630/e7n2j4tn/
[3]:	https://jsfiddle.net/eva19950630/ps7mxx1o/
[4]:	https://jsfiddle.net/eva19950630/h9gdtzg7/
[5]:	https://jsfiddle.net/eva19950630/ec9r8eqL/
[6]:	https://vuejs.org/v2/guide/components.html#Content-Distribution-with-Slots
[7]:	https://cythilya.github.io/2017/10/11/vue-component-slot/
[8]:	http://blog.tonycube.com/2017/05/vuejs-9-2-component.html
[9]:	https://ithelp.ithome.com.tw/articles/10185618
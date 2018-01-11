# Day19 - [Components] 動態元件(Dynamic Components)

上一篇的`slot`可以應用來把整個頁面分好三個section：navbar、main、footer，那如果我們要在main畫面上做tab來切換畫面呢？類似下圖的感覺：

<img src="picture/19_tab_example.png">

* [圖片來源](http://www.wix.com/website-template/view/html/1723?utm_source=freewebsitetemplates&utm_medium=template_banner&utm_term=design&utm_content=ma_html_fwt_temp_1_1_knollwalters&utm_campaign=ma_fwt&experiment_id=cpa_fwt_temp_1_1_knollwalters1723)

當我們想切換tab頁面時，Vue也簡化了寫法，這篇我們將介紹Vue.js提供的另一個寫在HTML中的特殊標籤元素`<component>`，它可以掛載元件，如果再綁定(`v-bind`)好它的`is`屬性，決定好它要載入的元件是什麼，如此一來，我們就可以實現動態綁定，把多個元件掛載在這個`<component>`上，然後動態切換不同元件。

範例：

```html
<div id="app">
    <ul class="navbar">
       <li><a href="#" @click="changeTab('home')">Home</a></li>
       <li><a href="#" @click="changeTab('about')">About</a></li>
       <li><a href="#" @click="changeTab('contact')">Contact</a></li>
    </ul>
    <hr>
    <component :is="currentView"></component>
</div>
```

```javascript
Vue.component ('home', {
    template: '<div>This is Home page.</div>'
})

Vue.component ('about', {
    template: '<div>This is About page.</div>'
})

Vue.component ('contact', {
    template: '<div>This is Contact page.</div>'
})

new Vue ({
    el: '#app',
    data: {
        currentView: 'home'
    },
    methods: {
        changeTab: function(v) {
            this.currentView = v
        }
    }
})
```

也可以用區域元件來做：

```javascript
new Vue ({
    el: '#app',
    data: {
        currentView: 'home'
    },
    methods: {
        changeTab: function(v) {
            this.currentView = v
        }
    },
    components: {
        home: {
            template: '<div>This is Home page.</div>'
        },
        about: {
            template: '<div>This is About page.</div>'
        },
        contact: {
            template: '<div>This is Contact page.</div>'
        }
    }
})
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/h9hdgh3k/)

### 用`keep-alive`暫存動態元件的狀態

如果我們希望在動態切換元件的時候，可以保留原本元件的資料或狀態，避免重新渲染，讓原本被切換掉的元件可以暫存在記憶體中，這時候可以使用`keep-alive`。

範例：試試看在範例的兩個input框內輸入內容，切換元件後，再切回home這個元件，看看有什麼變化。

```html
<div id="app">
    <ul class="navbar">
       <li><a href="#" @click="changeTab('home')">Home</a></li>
       <li><a href="#" @click="changeTab('about')">About</a></li>
       <li><a href="#" @click="changeTab('contact')">Contact</a></li>
    </ul>
    <hr>
    <component :is="currentView"></component>
    <keep-alive>
        <component :is="currentView"></component>
    </keep-alive>
</div>
```

```javascript
Vue.component ('home', {
    template: `<div>This is Home page.
                    <p><input type="text" placeholder="Type your name here..."></p>
                </div>`
})

Vue.component ('about', {
    template: '<div>This is About page.</div>'
})

Vue.component ('contact', {
    template: '<div>This is Contact page.</div>'
})

new Vue ({
    el: '#app',
    data: {
        currentView: 'home'
    },
    methods: {
        changeTab: function(v) {
            this.currentView = v
        }
    }
})
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/25wf2Lp0/)

日子一天天過，Vue我們也大致快介紹完一些比較重要、常用的功能或語法，慢慢感覺可以組合寫出一個完整網頁專案了，接下來我們認識完**Vuex**後，我們開始會實際做一些小專案，然後再學習怎麼和後端整合，最後完成一個比較完整的專案。

-----

### 參考資料
* [Vue.js - Dynamic Components](https://vuejs.org/v2/guide/components.html#Dynamic-Components)
* [Vue.js: 動態元件 Dynamic Components](https://cythilya.github.io/2017/10/22/vue-component-dynamic-components/)
* [Vue.js (9.2) - 元件(Component)](http://blog.tonycube.com/2017/05/vuejs-9-2-component.html)
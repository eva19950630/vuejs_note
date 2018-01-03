# Day14 - [Options] filters & computed

常常我們想要對資料做處理的時候，會寫一些function去跑，然後顯示出想要的結果，而為了讓模板(View)可以專注在顯示資料，Vue有設計兩個選項物件屬性，`filters`跟`computed`，可以在這些屬性裡面將資料格式化處理或有運算邏輯的function，如此一來，View不會包含這些function，版面看起來會簡潔許多。

## `filters`(過濾器)

Vue提供的`filters`可以讓你自定義過濾器，到了版本2.x，`filters`最主要的功能是將**文字資料格式化**，我們可以在`filters`中放入自己寫好的`function`，像是全部英文字母轉大寫、價錢數值的格式化等等。

#### `filters`的使用方法分為兩種：

1. mustache模板插值(mustache interpolations)
```html
{{ message | filterA }}
```

2. `v-bind`表達式(v-bind expressions)
```html
<div v-bind:id="message | filterA"></div>
```

#### `filters`的種類分為本地過濾器(local filter)與全局過濾器(global filter)：

1. 本地過濾器(local filter)

```html
<div id="app">
    <span>{{ text | toUpperCase }}</span>
</div>
```

```javascript
var vm = new Vue ({
    el: '#app',
    data: {
        text: 'hello vue'
    },
    filters: {
        toUpperCase(value) {
            return value.toUpperCase();
        }
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/cc77auLr/1/)

2. 全局過濾器(global filter)

用`filters`將資料的第一個字變成大寫，順便練習前面使用過的`v-model`。

```html
<div id="app">
    <input type="text" v-model="mytext">
    <p>{{ mytext | capitalize }}</p>
</div>
```

```javascript
Vue.filter('capitalize', function(value) {
    return value.charAt(0).toUpperCase() + value.slice(1);
});

var vm = new Vue ({
    el: '#app',
    data: {
        mytext: 'hello'
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/22bpnnk3/1/)

#### `filters`可以用`|`串聯：

```html
{{ message | filterA | filterB }}
```

#### `filters`是JavaScript函數，所以可以傳入參數：

```html
{{ message | filterA('arg1', 'arg2') }}
```

## `computed`(計算屬性)

假設我們把程式邏輯寫在View中...

```html
<div id="app">
    <p>原始訊息：{{ message }}</p>
    <p>反轉訊息：{{ message.split('').reverse().join('') }}</p>
</div>
```

這還只是將文字倒轉的功能寫在View當中，當邏輯一多一複雜下去，模板裡就會寫很多function，如此一來就違反讓View專注在顯示資料的規則，變得很不簡潔。

這個`computed`的功能很強大，可以對資料做處理計算，而且它有cache，能避免重複處理資料，跟`filters`一樣，也是在`computed`裡放入`function`，不同的是，`computed`可以做比較複雜的資料處理，資料處理完之後就將資料回傳。

`computed`可以透過`this`從`data`取得資料並拿來做運算，兩者有相依性，`data`資料改變，`computed`也會更新。

範例1：將文字倒轉過來

```html
<div id="app">
    <p>原始訊息：{{ message }}</p>
    <p>反轉訊息：{{ reverseMessage }}</p>
</div>
```

```javascript
var vm = new Vue ({
    el: '#app',
    data: {
        message: 'Hello'
    },
    computed: {
        reverseMessage() {
            return this.message.split('').reverse().join('');
        }
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/oap0gvur/)

範例2：使用`computed`做文章摘要

當我們做網頁，想把一段文字縮短成幾個字，後面部分以...代替，也可以使用`computed`來做。

```html
<div id="app">
    <p>原始內容：{{ content }}</p>
    <p>文章摘要：{{ summary }}</p>
</div>
```

```javascript
var vm = new Vue ({
    el: '#app',
    data: {
        content: '`computed`的功能很強大，它可以對資料做處理，而且它有cache，可以避免重複處理資料，跟`filters`一樣，可以在`computed`裡放入`function`，不同的是，`computed`可以做比較複雜的資料處理。'
    },
    computed: {
        summary() {
            if (this.content.length > 30)
                return this.content.slice(0, 27) + '...';
            return this.content;
        }
    }
});
```

* [run on JSFiddle](https://jsfiddle.net/eva19950630/4z5vgzae/)

## `filters` vs `computed`

* `filters`適合做簡單的文字格式處理。
* `computed`可用於做較複雜的邏輯運算處理。

## `computed` vs `methods`

比較 | `computed` | `methods`
------------- | ------------- | -------------
特徵 | `computed`會依賴屬性`data`中的資料，前面提到`computed`有cache，`computed`執行完的運算結果會暫存到cache，所以如果相依的資料不變，就不會重新計算，直接從cache回傳暫存資料。 | `methods`只要重新渲染元素呼叫到函式，就會重新計算一次。
回傳資料時機 | 從`data`取得資料並處理完後直接回傳給View顯示。 | 需在View呼叫函式才會執行。
適用時機 | 因為會暫存結果，所以適用運算結果資料不會改變時，效能也會比較好。 | 需要每次更新且不要暫存結果，適合狀態的改變。

-----

### 參考資料

* [Vue.js - Filters](https://vuejs.org/v2/guide/filters.html)
* [How to Create Filters in Vue.js with Examples](https://scotch.io/tutorials/how-to-create-filters-in-vuejs-with-examples)
* [Method vs Computed vs Filter](https://forum-archive.vuejs.org/topic/830/method-vs-computed-vs-filter)
* [Vue.js: Filter 過濾器](https://cythilya.github.io/2017/05/23/vue-filter/)
* [Vue.js (6) - 計算屬性](http://blog.tonycube.com/2017/04/vuejs-6-computed.html)
# Day04 - Virtual DOM & V-Node

## 認識Virtual DOM

**DOM**(Document Object Model)的中文翻成**「文件物件模型」**，是HTML、XML、SVG文件可以使用的一組API。它提供了文件結構化的表示法(樹狀結構)，並定義讓程式可以存取並改變文件架構、風格和內容的方法，目的是為了搭起網頁與程式碼之間溝通的橋樑，將網頁與程式碼(或script)連結在一起。

一個網頁的所有元素組織在一起，就構成了一棵**DOM Tree**。

那為什麼會出現Virtual DOM呢？

DOM最常用來跟JavaScript溝通，在瀏覽器中我們常用js來操作DOM，像是動態增加list項目、使用ajax回傳顯示訊息等等，這樣操作會讓整個頁面重新render一次，結果才會呈現出來，如此一來，對於整體運作來說是非常耗效能的，加上現在的網頁DOM Element數量龐大，更新元件的效能真的不高，因此為了**提高效能**，Facebook團隊在研發ReactJS時，改良MVC架構的View，就出現了Virtual DOM這樣的新技術。

**Virtual DOM**(虛擬DOM)是以JavaScript物件模擬特定DOM Tree，也就是不直接操作DOM，而是改用**模擬結構**的方式，達到優化效能的目的，讓頁面刷新載入的速度變快。

下面用一張圖來看看Virtual DOM的操作原理：

<img src="picture/4_virtual dom運作原理.png">

Virtual DOM不會讓瀏覽器掃描整個DOM Tree，也就是不會刷新整個頁面，它會使用`DOM diff`這個演算法去比較這一次跟上一次Virtual DOM的差異，接著處理有差異的部分，然後更新需要被更新的元件。

其實如果開發的頁面不多，不太需要用到Virtual DOM，因為可能會在實際上運作時跑比較慢，但是如果頁面比較多元素也多，那使用Virtual DOM就會讓整體效能變好。

## Vue.js實現的Virtual DOM：VNode

Vue在版本2.0之後才加入Virtual DOM，Vue的Virtual DOM是**VNode**，一個VNode的結構包含以下這些屬性：
* `tag`：該節點的html標籤
* `data`：該節點的數據資料
* `children`：該節點底下的子節點
* `text`：該節點的文本
* `elm`：當前虛擬節點對應的真實DOM節點
* `ns`：該節點的namespace
* `context`：編譯範圍
* `functionalContext`：函數化元件的編譯範圍
* `key`：該節點的key屬性，用來辨識該節點
* `componentOptions`：創建vue instance會用到的資訊
* `child`：該節點對應的vue instance
* `parent`：該節點的父節點
* `raw`：raw html
* `isStatic`：該節點是否為靜態節點
* `isRootInsert`：該節點是否作為根節點插入tree，值為false
* `isComment`：該節點是否作為註釋節點
* `isCloned`：該節點是否為克隆節點
* `isOnce`：該節點是否有v-once指令

而我們透過`new`實體化的VNode可分為以下幾類，下面為比較常見的五個分類，還有其他分類，就不細項列出。
* `EmptyVNode`：沒有內容的註釋節點
* `TextVNode`：文本節點
* `ElementVNode`：普通元素節點
* `ComponentVNode`：元件節點
* `CloneVNode`：克隆節點，可生成上面任意類型一模一樣的副本節點

在Vue.js的實作中，如果想透過JavaScript來操作元件，我們可以使用`render` function來操作Virtual DOM，後面會有一篇文章介紹`render` function的實作應用。

-----

### 參考資料
* [文件物件模型 (DOM)](https://developer.mozilla.org/zh-TW/docs/Web/API/Document_Object_Model)
* [Virtual DOM 概述](https://cythilya.github.io/2017/03/31/virtual-dom/)
* [用範例理解 Vue.js #4：Virtual DOM](https://ithelp.ithome.com.tw/articles/10191617)
* [[Node.js] 如何使用 Virtual DOM 實作 MVVM](https://oranwind.org/node-js-how-to-use-node-js-workflow-zhuang-tai-ji/)
* [Vue原理解析之Virtual Dom](https://segmentfault.com/a/1190000008291645)

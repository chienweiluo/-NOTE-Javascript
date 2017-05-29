# JavascriptNote

---

## vanillaJS

照著脈絡

`

1, 查詢取得DOM

2, 修改class屬性

3, 修改DOM

4, 監聽事件

5, 動畫

6, 包裝

`

### 1, 查詢取得DOM

querySelector() & querySelectorAll()

    // 取得單一元素   const oneElement = document.querySelector('#foo > div.bar')   
    // 取得所有符合的元素   const allElements = document.querySelectorAll('.bar')
    
getElementById() & getElementsByTagName() 仍可以很好的使用

**差別 querySelector 不能動態更新查詢到的元素。**

>把 querySelectorAll() 回傳的 NodeList 轉成 Array 之後， 就能用 forEach() 方式走訪每個元素。 <br/>
>  Array.from(allElements).forEach(element => { // do something... }) <br/>
><br/>
>// IE 還不支援 Array.from()，可以用： <br/>
>  Array.prototype.forEach.call(allElements, element => {   // do something... }) <br/>
><br/>
>// 更短的寫法： <br/>
>  [].forEach.call(allElements, element => {   // do something... })<br/>
><br/>
><br/>註: 還不是很懂這段的意思, 待查

-----

### 2, 修改class&屬性

類別(classList)

    element.classList.add('baz')  // 加入class baz

    element.classList.remove('baz')  //刪除 class baz

    element.classList.toggle('baz')  //toggle class baz

    element.classList.contains('baz')  //檢查是否含有class: baz

屬性

    const oneValue= oneElement.value  //取得屬性value

    oneElement.value=""hello"  //改變這個屬性內容為哈囉

設定多種屬性用 物件

    Object.assign(oneElment.{
        id: 'a',
        value: 'b',
        name: 'CCC'
    })
      
刪除屬性
  
    oneElement.value =null  //屬性內容為空
      
      
提到為何不用getAttribute() setAttribute() removeAttribute() ? 原因是這些方式是直接修改HTML屬性, 都會導致瀏覽器*重繪* , 對效能來說有很大的影響

除非是要修改的屬性真的需要重繪(例如表格德COLSPAN)例外

CSS樣式(行內)

    element.style.paddingTop = '2em'  //用駝峰
  
取得元素的css值

    getComputedStyle(element).getPropopertyValue('padding-top') // 這時又是一般的命名
      
-----

### 3, 修改DOM

-插入

在el1 裡插入一個el2

    el1.appendChild(el2)
    
在el1 裡的el3 之前插入el2

    el1.insertBefore(el2, el3)
    
在el1裡的el3**之後**插入el2

    el1.insertBefore(el2, el3.nextSibling)
    
-複製

    const newEl= oneEl.cloneNode()
    el1.appendChild(newEl)
    
-建立

        const newEl = document.createElement('div')
        
        const newTextNode = document.createTextNode('HELLOOOOO')

-移除

        parentElement.removeChold(el1)   //參照父元素移除DOM
        
        el1.parentNode.removeChild(el1)  //自己移除自己
        
        
-修改元素的內容


1, innerHTML


        element.innerHTML= '<div><p>HELLLOO<p></div>'


2, documentFragment


        const text = document.createTextNode('continue reading...')
        const hr = document.createElement('hr')
        const fragment = document.createDocumentFragment()

        fragment.appendChild(text)

        fragment.appendChild(hr)

        element.appendChild(fragment)
    
-----

### 4, 監聽事件

addEventListener(event, handler, options)

*觸發一次(once)

    element.addEventListener('change', function listener(event){
      console.log(event.type +'got triggered on' + this)
      this.removeEventListener('change', listener)
    })

-----

### 5, 動畫

setTimeout(),

window.requestAnimationFrame()

-----

### 6, 包裝

最後我們可以把這些方式全部包在一個 function 裡。
就像 jQuery 一樣，還可以鍊式呼叫（chainable）
（例如： $('foo').css({color: 'red'}).on('click', () => {}) ）

    const $ = function $(selector, context = document) {
      const elements = Array.from(context.querySelectorAll(selector))
      return {
        elements,

        html (newHtml) {
          this.elements.forEach(element => {
            element.innerHTML = newHtml
          })
          return this
        },

        css (newCss) {
          this.elements.forEach(element => {
            Object.assign(element.style, newCss)
          })
          return this
        },

        on (event, handler, options) {
          this.elements.forEach(element => {
           element.addEventListener(event, handler, options)
          })
          return this
        }
      }
    }

或者用 ES6 的 Class 來包裝：

      class DOM{ 

      constructor(selector) { 
        const elements = document.querySelectorAll(selector) 
        this.length = elements.length 
        Object.assign(this, elements) 
      } 

      each(callback) { 
        for (let el of Array.from(this)){ 
        callback.call(el) 
        } 
        return this 
      } 

      addClass(className) { 
        return this.each(function (){ 
          this.classList.add(className) 
          }) 
      } 

      removeClass(className) { 
      return this.each(function () 
        { this.classList.remove(className) }) 
      } 

      hasClass(className) { 
        return this[0].classList.contains(className) 
        } 

      on(event, callback) { 
       return this.each(function () { 
       this.addEventListener(event, callback, false) 
       }) 
      }    

      // etc. }

-----


參考資料, 範例: 
[The Basics of DOM Manipulation in Vanilla JavaScript (No jQuery)](https://www.sitepoint.com/dom-manipulation-vanilla-javascript-no-jquery/),[ [心得] 都2017年了  學學用原生JS來操作DOM吧](https://www.ptt.cc/bbs/Web_Design/M.1491563726.A.508.html),[W3Cschool],[菜鳥教程]
,[MDN]


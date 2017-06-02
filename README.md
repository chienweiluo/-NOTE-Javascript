# JavascriptNote

---

## ES6的 let/ const/ var  


在解釋之前, 要先知道JS的 `提升(Hoisting)` 的特性

>变量声明提升(Variable hoisting)<br/>
>JavaScript 变量的另一特别之处是，你可以引用稍后声明的变量而不会引发异常。这一概念称为变量声明提升(hoisting)；
>JavaScript 变量感觉上是被“提升”或移到了所有函数和语句之前。然而提升后的变量将返回 undefined 值。<br/>
>所以在使用或引用某个变量之后进行声明和初始化操作，这个被提升的引用仍将得到 undefined 值。<br/>
>取自<MDN 語法和數據類型> <br/>

也就是說, 用var等的變量表明符號時, 會將這行程式碼放到程式的最上面, 而因為還沒有賦予任何值, 所以會log出‵undefined‵

(let/const其實也會, 但死區暫不討論)

    var myvar = "my value";

    (function() {
      console.log(myvar); // undefined
      var myvar = "local value";
    })();
    
    //就等於
    
    var myvar = "my value";
 
    (function() {
      var myvar;
      console.log(myvar); // undefined
      myvar = "local value";
    })();
    
    // var被提升到塊中的最上方, 所以 undefined


函數提升: `对于函数，只有函数声明会被提升到顶部，而不包括函数表达式。`

函數聲明

    foo() //ok~

    function foo(){...}
   
    
函數表達式

    foo1(); // not a function  <--因為不被視為函數, 是被視為用var表明的一個變量

    var foo1 = function(){...}


---

ES6之前 JS只有定義變數沒有定義常數的方法


變數:
var變數可以儲存的值包含了各種的資料型態，包括基礎資料型態，物件。

變數算是暫時存放的容器

基礎資料型態包含了數值，字串，布林。物件包含內建物件與自訂義物件。


常數:

常數的值只能定義一次，因此在定義時就必須對它初始化，並且不能改變。
所以常數可以說是一旦存入就無法改變的資料, 是永久存放的

var

任何不使用var關鍵字的變數，都會被視為全域變數。


const

const是針對常數的定義

常數一開始宣告的時候就必須要指定給值

    const a = 1
    a = 20 //TypeError

const也可以用來指定 Object 或 Array 做常數宣告

    const a = []
    a[0] = 1
    
    const o ={}
    o.prop = 111

這個識別名稱的參照是唯獨的, 

但是!

注意的是, 這個參照指定到的值是可以改變的

---

let,const是區塊作用域 

let 與const 宣告就是以區塊語句為分界的作用域

函式作用域var

但雖說函式作用域, 其實在一些區塊語句, if,else,for,while, 使用var宣告的變數仍然會曝露到全域之中可被存取, 然後如果再加上原本根本搞不懂Hoisting, 他媽的我根本不知道結果會跑出什麼啊!!!

所以有些文檔會建議把所有var語句放到程式碼的最上面(連for裡面的都要喔)


參考資料: 
    [維克的煩惱](http://www.victsao.com/blog/81-javascript/83-javascript-variate-const),[從ES6開始的Javascript學習生活](https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part3/var_const_naming.html), [Day 05: ES6篇 - let與const ](http://ithelp.ithome.com.tw/articles/10185142),[MDN變量](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Grammar_and_types#Array_literals)



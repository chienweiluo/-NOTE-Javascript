# JavascriptNote

## JS原型鍊

---

1.

`__proto__`: 對象__proto__屬性就是他所對應的原型對象(所有對象都有__proto__)

>Every JavaScript object has a second JavaScript object (or null ,<br/>
>but this is rare) associated with it. This second object is known as a prototype, <br/>
>and the first object inherits properties from the prototype.<<br/>
>   from book: Javascript權威指南

每個js對象一定對應一個原型對象, 並從原型對象繼承屬性和方法, 如何對應? 答案就是 __proto__

像是如果調用了a.country(下面Person的例子), 發現a對象裡面並沒有這個屬性, js就會從__proto__, 一直往上(原型鍊)查找, 如果有的話就使用country屬性, 如果沒有的話就直到Object.prototype 為止, 因為以上就會 return null了

2.

`prototype`: 只有函數才有的prototype屬性, 因為沒有class的概念, js就用 function 來模擬類似的概念.

當你創建一個函數的時候, JS會自動為你的函數新增prototype屬性(對應到的是空對象), 而一旦把這個函數以new 調用, js就會幫你新建一個這個構造函數的instance,  

而這個instance 繼承了構造函數中的所有屬性 和 方法, 

也可以這麼說: 這個instance 通過設置自己的__proto__指向承構造函數的prototype來實現這種繼承

3.

Javascript 裡面所有的數據類型都是對象Object, 所以繼承以及原型鍊的就是為了將所有的Object聯繫起來而生的



---

JS 在ES6之後雖然號稱有了完整 OO的架構, 但還是沒有class(es6的class 相似語法糖), 但確實是有個類似的機制來實現

作者在設計`extend` 的時候, 不像其他語言有`class`的觀念, 是通過`new` 一個`constructor` 來實現

`new + 構造函數`

    //呼叫constructor
    
    function Person(name, age){
      this.name= name;
      this.age= age;
    }
    
    Person.prototype = {
        country: 'China'
    }

    var nick = new Person('nick',18)
    var peter = new Person('peter',18)
    
    console.log(nick.name)    //nick
    console.log(peter.name)   //peter
    console.log(nick.country)    //China
    console.log(peter.country)    //China



裡頭Person 就是構造函數, 可以用new 出一個instance

但是如果在Person 裡頭加入一個function, 在name 跟 age這兩個屬性下, 很明顯是每一個instance(nick, peter)都會不一樣, 但是剛剛加入的function卻是在做同一件事情(但是其實還是佔了兩份空間, 也就是根本是不同的兩個function)

所以就把這個function 抽了出來, 變成所有Person都可以共享的方法.

因為構造函數有一個缺點就是沒辦法共享屬性和方法, 所以......

要共享屬性和方法的話, 就要用到 `prototype` 了

所以就想到一個把function綁到 `Person.prototype` 上面, 以讓所有Person的 instance 都可以共享這個辦法

`
所有實力最對象需要共享的屬性和方法就會放到.prototype裡, 而不需要共享的就會放在構造函數constructor <br>
所以只要更動prototype對象, 就會同時影像到所有instance

`

>你有一個叫做`Person`的函數, 就可以把`Person`當作constructor, <br/>
>利用`var obj = new Person()`來new出一個`Person` 的instance,<br/>
>並且可以在>`Person.prototype`上面加上你想讓所有instance共享的屬性或方法<br/>

通過Person函數所new出來的對象, 都自動有一個__proto__屬性, 它的值是指向Person.prototype的, Person.prototype是直接由Object生成 , 但Person.prototype.__proto__也是一個對象, 所以說, 最後就會指向Object.prototype, 這也就是原型鍊的頂端.

tips: 這時候使用this關鍵字, 就會指向新建造的實例nick,peter

 a: 實例 peter,nick<br>
 Person: 函式 (new 出 instance nick,peter)<br>

<b>所以在這裡組成的原型鍊:
    a.__proto__ == Person.prototype ,  [ a的原型對象指向了Person的原型對象 ]<br/>
    a.__proto__.__proto__ == Person.prototype.__proto__ ,<br/>
    Person.prototype.__proto__ == Object.prototype [Person的原型對象的原型對象就是Object的原型對象]<br/></b>

在這個例子中可以知道: <br>
instance的__proto__就是Function.prototype(因為是function把它製造出來的)<br>
Function的__proto__就是製造它出來的Function.prototype.<br>
Function.prototype的__proto__ 會指向 Object.prototype<br>
<br>
證明在js中function的地位真的是很大......<br>

-----

<b>為什麼一開始要提到prototype跟__proto__這兩個饒口的東西呢? </b>

因為Javascript正是使用這兩個東西合作來實現了原型鍊以及對象的繼承! 

而構造函數, 是通過prototype來儲存將來要共享的屬性和方法, 也可以設置prototype來指向現存的對象來繼承被繼承的對象

對象的__proto__指向自己構造函數的prototype。

`obj.__proto__.__proto__...`的原型鍊就此產生, 包括操作符instanceof正是通過探測`obj.__proto__.__proto__... === Constructor.prototype`来验证obj是否是Constructor的实例。

    var one = {x: 1};
    var two = new Object();
    one.__proto__ === Object.prototype // true
    two.__proto__ === Object.prototype // true
    one.toString === one.__proto__.toString // true

two = new Object()中Object是構造函數<br/>
所以two.__proto__就是Object.prototype<br/>
最後one，ES規範定義對象字面量的原型就是Object.prototype。<br/>


>總結：先有Object.prototype (鍊頂端) , Function.prototype繼承Object.prototype而生<br/>
>最後, Function和Object和其它構造函數繼承Function.prototype生。<br/>

所以原型鍊可以幹嘛? 可以呼叫parent的 method&proprerty, 也就是繼承啦!

<img src="http://www.mollypages.org/tutorials/jsobj_full.jpg">



參考資料: [creeperyang大神的github](https://github.com/creeperyang/blog/issues/9), [TB技術共筆huli大](http://blog.techbridge.cc/2017/04/22/javascript-prototype/?utm_source=tuicool&utm_medium=referral), [Jason大-聽你blog](http://www.jasonsi.com/2017/03/15/36/), [mollypages](http://www.mollypages.org/tutorials/js.mp)

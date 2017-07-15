# JavascriptNote

## JS原型鍊 - 創建對象

(其實這一篇也完完整整是原型鍊, 也就是原型繼承, 但如果都打在同一篇怕有篇幅太長的疑慮, 就想把創建對象的方式獨立一個branch.)

### 先知道

1. 在JS可以使用構造函數/原型鍊在創建對象, 最好的方法當然是各取各好的地方

2. 繼承: 為面向對象的語言(OO)的基礎(封裝、繼承、多型 >順序也是如此)之一, 大約思路大概是這樣

造物主創造前100人的時候, 發現同樣的事情竟然要做100次有夠麻煩, 於是就觀察這100個人, 發現了每個人都有共同的地方, 


[造物工廠時代] 于是做了一個機器 將這個機器設定功能(共同的特點), 再加點個性(每個人不同)的方法, 這樣造人就方便多了

接著, 用這個機器要創造動物的時候, 發現動物的某些設置(例如陸上動物"奔跑"不夠快)錯了, 因為沒有解決`對象識別`的問題, 就不得已只能把所有動物都改一遍

V

[繼承物種時代] 這時候發現人跟動物也有很多的相同點, 但又不太一樣, 所以要進行更高層的`抽象`(更詳細的將相同點抽出來), 於是造物主抽象出來基本的類別 動

物模板, 以這個類別繼承出陸上動物模板/海上動物模板, 各有各的特點, 像陸上動物有奔跑, 海上動物有游泳

而回到原本造人的動作, 人屬於陸上動物, 所以人類模板繼承了陸上動物模板, 並添加各種人類共同的屬性, 像是站立行走或感情等等

之後開始創造人這個個體, 因為已經有了人類模板, 所以只有基於這個人類模板, 創造各個實例, 然後對這個實例添加人的各種個性

于是這樣解決了重複製造的麻煩性, 在也解決了對象識別的問題, 這就是繼承


統整是:  將相同會使用到的屬性或方法先`抽`出來, 定義高度抽象的基本類別, 然後在`繼承基本類別擴展`出更詳細的子類別, 然後就是不斷得更具體, 直到可以創建`各種`個體, 某種程度相似于剛剛提到的 封裝> 繼承> 多型 的動作

> 主要的目的: 封裝: 降低互相依賴的程度, 繼承: 程式碼再用, 多型: 避免重複的工作, 站在巨人的肩膀上 
> - 提高可維護性

4. 所有函數的默認原型都是 Object 的instance，因此默認原型都會包含一個內部指針，指向 Object.prototype。

        function HEY(){
            console.log("hey")    
        }
        HEY.prototype.__proto__ == Object.prototype

5. 基礎型別: Boolean/String/Number/Undefined/Null

其實不用知道也可以往下看ㄎㄎ

##### 構造函數

引用類型: Object/ Array/ Function/ Date/ RegExp

=> 生成instance, 也就是`對象定義`

用new 生成instance 就是構造函數

> 以上面的引用類型: var a = new Object() <= 相同 => var a = {} 

>new 構造子做的事??

        function newObj(cons,args){
            var O = new cons(); // 創造一個新的Object O
            O.__proto__ = cons.prototype; //O繼承原型鍊得到所有屬性及方法
            cons.apply(O, args);  //讓this指向O
            return O;  // return O
        }        

**

函數是在特定環境中執行程式的對象, 因此使用apply/call方法可以在新創建的對象上執行構造函數

而這裡算是借用構造函數來讓this指向超類別

        function SuperType(){
            this.colors = ["red", "blue", "green"];
        }

        function SubType(){
            SuperType.call(this);
        }

        var instance1 = new SubType();
        instance1.colors.push("black");
        console.log(instance1.colors); ["red", "blue", "green", "black"]

        var instance2 = new SubType();
        console.log(instance2.colors); ["red", "blue", "green"]

解決了原型無法向超類傳遞參數的問題 子類的不同實例也不會互相影響

但

缺點: 

1. 在構造函式中的實例, 會將function也複製N 遍造成浪費資源(無法複用)

        function Person(){
            this.name= name;
            this.age= age;
            this.sayName= function(){
                console.log(this.name)
            };
        }

        var p1= new Person()
        var p2= new Person()

        p1.sayName ==p2.sayName 
        //false
        //但卻是一樣的方法, 造成資源浪費

2. 超類別定義的方法對子類來說是不可見的

##### 原型繼承

解決資源浪費的方法就是原型prototype

X.prototype --> X生成的所有instance 可共享X 的屬性及方法

註: X 是funcion, function類型都有一個屬性prototype 而相似概念的 __proto__ 我解釋為指針指向原型

        person1 = new Person()

        person1.__proto__ == Person.prototype //true

缺點: 

1. 一個instance改變, 其他instance也受到影響改變

2. 子類無法向父類以上傳遞參數

*通過原型實現繼承的時候, 不可以使用對象 {} 操作, 這樣會重寫原型鍊

        ...
        Son.prototype = new Dad()    
        Son.prototype = {...}
        //NONONONONONONONONONONONO
        


##### 組合繼承

> 使用原型鍊繼承實現對原型屬性和方法的繼承而通過構造函數來實現對實例屬性的繼承

> 構造函數用於定義實例屬性

> 原型模式用來定義方法或所有需要定義的共享屬性

        function SuperType(name){
            this.name  = name;
            this.colors = ["red", "blue", "green"];
        }

        SuperType.prototype.sayName = function(){
            console.log(this.name);
        };

        function SubType(name, age){
            SuperType.call(this, name);
            this.age = age;
        }

        SubType.prototype = new SuperType();

        SubType.prototype.sayAge = function(){
            console.log(this.age);
        };

        var instance1 = new SubType("luo",22);
        instance1.colors.push("black");
        console.log(instance1.colors);  //["red", "blue", "green", "black"]
        instance1.sayName(); //"luo"
        instance1.sayAge(); //22

        var instance2 = new SubType("chienwei",22);
        console.log(instance2.colors); //["red", "blue", "green"]
        instance2.sayName(); //"chienwei"
        instance2.sayAge(); //22



參考資料: [于江水大大的原型鍊相關文章](http://yujiangshui.com/)

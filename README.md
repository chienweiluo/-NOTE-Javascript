# JavascriptNote

---

## call apply bind

### 先提到call/apply

JS是動態語言, 可以動態變換運行上下文, 而CALL/APPLY 就是在Func.prototype實現變換的方法, 所以說是每個方法都有這兩個屬性

> 兩者的作用一樣, 意即改變this的指向

差別在於 

> 傳遞參數的方式不同(call 是一個一個地傳遞參數, apply 是將參數包在一個array裡)

    foo(){
    ...}

    foo.call(this1, arg1, arg2 ...)

    foo.apply(this1,[arg1, arg2...])

    *this是你想指定的上下文, 本身是沒有foo的方法的, 但調用call/apply後 就可以調用foo的方法了

**換句話說, 就是將foo(當前的this)綁定到this1, 這時候this1具備了foo的屬性和方法, 或是說this1繼承了foo的屬性和方法**

example1:

實現對象繼承

    var Parent = function(){
        this.name= "Chienwei";
        this.age = 22;
        this.pet = "dodo"
    }

    var child= {};

    console.log(child) // 空對象

    Parent.call(child);

    console.log(child)

    // Object{
      name= "Chienwei";
      age = 22;
      pet = "dodo";
      } 

example2: 

‵

    function talk(a,b,c,d){
        alert(a+b+c+d);
    }

    function yell(a,b,c,d){
        //call
        talk.call(this, a, b, c, d);
        //apply
        talk.apply(this, arguments)
        //等於
        talk.apply(this, [a,b,c,d])
    }

    yell("大","聲","就","贏")
    //大聲就贏

‵

> 參數明確的時候使用call, 不確定的時候用apply

-----


### bind

`obj.bind(thisObj,arg1, arg2)`

> 把obj绑定到thisObj，这时候thisObj具备了obj的属性和方法

> 如果bind的第一个参数是null或者undefined，等于将this绑定到全局对象。

bind() 




參考資料: 
[http://www.cnblogs.com/fighting_cp/archive/2010/09/20/1831844.html](http://www.cnblogs.com/fighting_cp/archive/2010/09/20/1831844.html)
,[如何理解和熟练运用js中的call及apply？](https://www.zhihu.com/question/20289071)
,[js笔记——call,apply,bind使用笔记](http://www.cnblogs.com/52fhy/p/5118877.html)



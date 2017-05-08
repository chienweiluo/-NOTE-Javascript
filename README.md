# JavascriptNote

>EC:　excution context       函數執行環境 <br>

當控制器轉到ECMAScript碰到可執行代碼的時候 就會進入到 EC

這裡提到了 可執行代碼 , 可執行代碼有三種類型

1. GLOBAL <br/>
  像是一個環境, 像是看到HTML裡面的 <script> 標籤, 或是加載external 的js代碼, 這個階段是不包含裡頭的忍和function 程式碼, 一旦引入後, 第一個接收到的代碼就是這個環境

2. FUNCTION<br/>
  就是函數. tips: 具體的函數體內的代碼並不包括內部函數的代碼 
  
3. EVAL<br/>
  eval()內的代碼
  
而EC 的建立包含了 1. 創立 2. 執行 兩個階段<br/>

1. 創立 <br/>
  指的是已經被調用, 但還未執行任一程式碼的狀態<br/>
  a. 創建scope chain<br/>
  b. 創建 var, function, arg<br/>
  c. 求得this 的值<br/>
  
2. 執行<br/>
  初始化變數的值和函數引用, 執行程式碼. 

>ECS: excution context stack 執行環境的堆疊(tips: stack 後進先出) <br/>

因為瀏覽器渲的js 是為單線程, 所以代表了一次只能做一件事情, 其他的行為都會被放到ECS的頂部排隊

當你調用了一個函數, 時間點就會進入那個被調用的函數, 然後創建一個新的EC, 並將這個新的EC放到Stack的頂部, 因為是堆疊的關係, 如果調用函式內部的一個函式, 裡頭的函式就會先跑到頂部先執行, 最後執行完就回到永遠在最底部的global函式

時序 順排

`

    4. EC3   -> current
    3. EC2   -> 之後1
    2. EC1   -> 之後2
    1. global-> 永遠在最底(後進先出)
    
`



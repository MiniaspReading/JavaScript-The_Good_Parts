# 第四章 函式
函式是 JavaScript 的基礎模組單位。用於程式碼的**再利用**、**資訊的隱藏**和整體的結構，程式設計的精義，就是如何把**一組需求**分解成一組函式與資料結構。

## 函式物件
JavaScript 的函式都是物件，物件是許多『名稱/值』的集合，與 prototype 物件與有聯繫。函式物件則會聯繫到 Function.prototype(本身再聯繫到 object.prototype)。
每個函式也會建立兩個隱藏特性：含是的背景情景與實作函式行為的程式碼。這個函式物件也一併建立一個 prototype 特性，這個特性的值具有 contructor 特性的物件。

函式能儲存再變數、物件或陣列裡。函數能被當成引數傳給另一個函式，函式也能被另一個函式回傳，函式既然是個物件，他也可以有自己的方法。

## 函式實字

函式物件以函式實字建立：
```sh
var add = function (a,b){
    return a + b;
};
```
函式有四個部份：   
  * 第一部份：保留字 function 
  * 第二部份：可選用的函式名稱。可以使用自己的名稱，反覆自我呼叫。如果沒有名稱，則稱為**匿名函式**（anonymous function）。
  * 第三部份：函式參數的集合，需要以括號圍起，可以由零到多個名稱並以逗號間隔，參數根據函式被呼叫時提供的引數而初始化。
  * 第四部份：以一組大括號圍起的敘述。敘述是函式的本體，被呼叫時執行。

函式能被定義在另一個函式裡，內層的函式能取用自己的參數與變數，也能享有外層函式的參數和變數。這個對外圍環境的聯繫，稱為**閉包**（closure）。

## 呼叫

呼叫函式，將會暫緩現有的執行，把控制權和參數傳給新呼叫的函式。除了已宣告的參數，每個函式均接受兩個額外參數:this、argument。this 參數的值由**呼叫模式**決定。JavaScript 有四種呼叫模式。
  * 方法呼叫模式
  * 函式呼叫模式
  * 建構式呼叫模式
  * apply 呼叫模式
  
函式呼叫是用一對小括號，接在任何產生函式值得運算式後面。當引數量與參數量不相等時，不會產生執行的錯誤，多餘的會被忽略，不足的已 undefined 取代。引數值不會型別檢查，任何型別的值都能傳給參數。
 #### 方法呼叫
 當函式儲存成物件的特性時，稱為**方法**，呼叫方法時，this 即與物件相連繫。如果呼叫的運算式包含精確式（. 點號運算子、陣列註標(索引Index)運算式），則呼叫時視同方法。
 
 ```sh
 var myObject = {
     value : 0;
     increment : function (inc){
     this.value += typeof inc === 'number' ? inc : 1;
     }
 };
 
 myObject.increment();
 document.writeln(myObject.value);    // 1
 
 myObject.increment(2);
 document.writeln(myObject.value);    // 3
 ```
方法可使用 this 存取物件，能從物件中擷取值，或是調整物件，this 對物件的繫結（binding）發生在*呼叫期間*。繫結發生時期較晚，讓使用 this 的函式能受到高度重複利用。
利用 this 取的環境變數的方法稱**public method**。

 #### 函式呼叫
 當函式不是物件的特性時，則像函式一般被呼叫
 ```sh
 var sum = add(3,4);    // sum = 7
 ```
 這種呼叫模式，this 將結合至全域變數;這點式語言設計上的錯誤。這項錯誤導致方法無法列用內層函式協助工作，因為內層函式並未享有方法對物件的存取，他的 this 結合到錯誤的地方了。
 解決方式就是定義一個變數，並指派他的值為 this ，內層就可以透過這個變數存取 this 。
 ```sh
 myObject.double = function (){
          var that = this;   // 解決途徑
          var helper = function (){
              that.value = add(that.value,that.value)
          };
          helper();    //把 helper 視為函式而呼叫
 };
 myObject.double();
 document.writeln(myObject.getValue());  // 6
 ```
  #### 建構式呼叫
  Javascrip 是個原型繼承的語言，就是物件可以直接繼承其他物件的特性。這個語言不用類別。
  如果函式呼叫時加上字首詞 new ，建立的新物件將附有對函式的 prototype 成員值的隱藏聯結，而且** this 也將與新物件結合**。
  字首詞 new 也改變了 return 行為，後面會提到。
  ```sh
  // 建立成為 Quo 的建構式，讓他具有status 特性
  var Quo = function (string){
      this.status = string;
  };
  
  //為所有 Quo 的實例，賦予一個 get_status 的 public 方法。
  Quo.prototype.get_status = function (){
      return this.status;
  };
  
  //製作一個 Quo 的實例
  var myQuo = new Quo("confused");
  document.writeln(myQuo.get_status());  //  confused
  ```
  與字首詞 new 合用的函式，稱為**建構式**（consrtuctor）。這種風格的建構式並部建議使用，後面有更好的替代方案。
  #### apply 呼叫
  因為 JavaScript 是個物件導向語言，函式下可以有方法。
  
  apply 方法讓我們建構一個引數陣列，用於呼叫函式，也能讓我們使用 this 的值。   
  apply 方法接受兩個參數，第一個是應該與 this 聯繫的值，第二個則為參數的陣列。
  ```sh
  var array = [3,4];
  var sum = add.apply(null,array);  //sum = 7
  
  //製作一個具有 status 成員物件
  var statusObject = {
      status : 'A-OK'
  };
  
  // statusObject 分繼承 Quo.prototype 繼承而來
  // 但我們可以在 statusObject 上呼叫 get_status 方法呼叫
  // 儘管 statusObject 並不擁有 get_status 方法
  var status = Quo.prototype.get_status.apply(statusObject);    // status = 'A-OK'
  ``` 
## arguments 陣列
函式能在呼叫時獲取的額外參數，即為 arguments 陣列。他讓函式取用所有在呼叫時候提供的引數，**包括並未指派給參數的超額引數**。
```sh
var sum = function (){
    var i,sum = 0;
    for (i = 0; i < arguments.length; i += 1){
    sum += arguments[i];
    }
    return sum;
};
document.writeln(sum(4,8,15,16,23,42));  //  108

```
上例並非好用的模式，後面會在講到。因為設計上的錯誤 **arguments 並非真正的陣列，他是個像陣列的物件**，他具有 length 特性，但缺乏所有陣列方法。
## 回傳
return 敘述能讓函式提早回傳，執行 return 時，函式立刻回傳，不會執行後續的敘述。若未指定 return 值，則回傳 undefined 。

如果函式呼叫時附有字首詞 new ，而且 return 值不是物件，則改為回傳 this (新物件)。

## 例外狀況
JavaScript 提供一種例外事件(exception) 處理機制。例外事件是不尋常的災難(但並非不再預期中的)，會干擾程式的正常流向，偵測到此類災難，程式應該丟出例外事件。
```sh
var add  = function (a,b){
    if (typeof a !== 'unmber' || typeof b !== 'unmber'){
       throw {
           name : 'TypeError',
           message : 'add needs numbers'
       }
    }
    
    return a + b;
};
```
throw 敘述負責中斷函式的執行，他應該要拿到一個 exception 物件，物件中包含辦識例外類別的 name 特性，以及描述例外性質的 message 特性，也可以在家齊他的特性。
```sh
var try_it = function () {
    try {
        add("seven");
    } catch (e) {
        document.writeln(e.name + ':' + e.message);
    }
};
try_it();
```
如果在 try 區塊中丟出例外事件，控制權將轉交到 try 的 catch 子句。

## 擴充型別
JavaScript 允許基本型別的擴充。擴充 Function.prototype 增加一個方法到所有函式下：
```sh
Function.prototype.method = function (name,func){
    this.prototype[name] =func;
    return this;
};
```
為 Function.prototype 添加 method 方法後，就不再需要鍵入 prototype 特性的名稱。
JavaScript 沒有獨立的整數型別，所以有時需要單獨抽離數值中整數部份。底下我們擴充一個 interger 方法。
```sh
Number.method('interger',function () {
   return Math[this < 0 ? 'ceil' : 'floor'](this);
});

document.writeln((-10 / 3).interger());  // -3
```
在混雜函式庫時務必小心，只在缺少方法時才擴充，則是個防禦技巧。
```sh
Function.prototype.method = function (name , func){
    if(!this.prototype[name]){
        this.prototype[name] = func;
    }
};
```
在 for in 敘述與原型互動很差（效能差），可以使用 hasOwnProperty 方法遮擋被繼承的特性。

## 遞迴

遞迴函式(recursive function)，是個直接或間接呼叫自己的函式。有待解決的大問題，被分解成相似的子問題集合，每個子問題都有一項較小的解決方案。
一般而言，遞迴函式會自我呼叫已解決子問題。

遞迴函式可以分常有效率的操作樹狀結構（如瀏覽器的 DOM），每次遞迴都交出一小部分。
```sh
// 定義 walk_the_DOM 函式，函式從指定的節點開始，
// 依 HTML 來源碼的順序，參訪每個數節點
// 呼叫一個函式，把函式逐一傳給每個節點
// walk_the_DOM 也呼叫自己，以處理每個子節點

var walk_the_DOM =function walk(node, func){
    func(node);
    node =node.firstChild;
    while (node){
        walk(node, func);
        node = node.nextSibling;
    }
};

// 定義 getElementsByAttribute 函式
// 接受屬性名稱字串，與選用的比對值
// 呼叫 walk_the_DOM 並把一個在節點中比對屬性名稱的函式傳過去
// 找到的節點則累積在 results 陣列中

var getElementsByAttribute = functuon (att, value) {
    var results = [];
    
    walk_the_DOM(document.body, function(node){
        var actual = node.nodeType === 1 && node.getAttribute(att);
        if (typeof actual === 'string' && (actual === value || typeof value !== 'string')){
           results.push(node);
        }
    
    });
    return results;
};
```

## 範圍
在程式語言中的**範圍**（scope），控制了變數與參數的可見與生命週期。他減少的命名的衝突，並提供了自動記憶體管理機制。
```sh 
 var foo = function () {
     var a = 3, b = 5;
     
     var bar = function () {
         var b = 7, c = 11;
         
         // a = 3, b = 7 ,c = 11
         a += b + c;
         
         // a = 21, b = 7, c = 11
     };
     // a = 3, b = 5, c = 未定義
     bar();
     // a = 21, b = 5
 };
```
JavaScript 的確具有函式範圍，定義在函式裡的參數和變數，含是以外即不可見，在內即可在函式內的任何地方都可見到這個變數。
在 JavaScript 中，最好在函式中本體的底端，宣告所有函式所用到的變數。 

## closure
之前範例在 getElementsByAttribute 函式能運作的原因，在於他宣告了 results 變數，而且他傳給 walk_the_DOM 的內層函數也能取用 results 變數。
發生內層比外層函式生命週期更常的時候。

之前範例製作了 myObject，他有 value 與 increment 方法，現在示範如和保護值，不被未授權的來源隨意改變。將透過**呼叫一個傳回物件實字的函式**而初始化 myObject。
```sh
var myObject = function () {
     var value = 0;
     
     return {
          increment : function (inc){
             value += typeof inc === 'number' ? inc : 1;
          },
          getValue : function () {
             return value;
          }
     }
 }
 }();

```
這個函式定義了 value 變數，仍可被 icrement 與 getValue 方法取用，但函式範圍把他隱藏起來，不然程式的其餘部份見到。並非把函式本身指派給 myObject，而是把函式的結果指派過去。

之前範例提過 Quo 建構式，他會產生具有 status 特性與 get_status 方法的物件。底下定義一個不同的 quo 函式。
```sh
var quo = functon (status) {
    return {
        get_status : function () {
            return status;
        }
    };
};
// 製作一個 quo 實例

var myQuo = quo("amazed");
document.writeln(myObject.get_status());
```
上例中的 quo 函式，不需要字首詞 new ，當我們呼叫 quo，他回傳包含 get_status 方法的新物件。對該物件的參考則儲存在 myQuo 。
get_status 方法仍有對 quo 的 status 特性的優先存取權，盡管 quo 已經被回傳了。
get_status 對**參數副本沒有存取權**，而是對**參數本身有存取權**。因為函式能取用建造他本身的背景環境，才有這種可能，這種狀況稱為**閉包**（closure）。

再實作一個範例，定義一個把 DOM 節點的背景設為黃色，再逐漸變成白色。
```sh
var fade = function (node)
{
    var level = 1;
    var step = function () {
        var hex = level.toString(16);
        node.style.backgroundColor = '#ffff' + hex + hex;
        if (level < 15) {
            level += 1;
            setTimeout(step, 100); 
        }
    };
    setTimeout(step, 100);
};

fade(document.body);
```
內層函式直接取用外層函式的變數，而不是取用變數副本。

## 回呼
函式讓不連續事件的處理更容易。網路上的同步性請求，使得用戶端處於『凍結』狀態。較佳的方式則是製作非同步請求，提供回呼函式，在收到伺服器回應才呼叫這個函式，所以用戶端不會被阻塞。
```sh
 request = prepare_the_request();
 sent_request_asynchronously(request, function (response) {
      display(response);
 });
```
對 sent_request_asynchronously 函式傳送一個函式參數，該函式參數將於接收到回應時被呼叫。

## 模組
我們可以使用函式與 closure 製作模組，模組式個函式或物件，用於呈現一個介面，但隱藏起他的狀態與實作，藉由使用函式產生模組，幾乎可以消除全域變數的使用，限制了 JavaScript 最糟的功能之一。

我們想為 String 擴充一個 deentityify 方法，他的功能是在字串中尋找 HTML tag 並用意義相同的字取代。
```sh
String.method('deentityify',function () { 
    
    var entity ={
        quot: '"',
        lt: '<',
        gt: '>'
    };
 return function () {
     return this.replace(/&([^&;]+);/g,
     function (a, b) {
          var r = entity[b];
          return typeof r === 'string' ? r : a;
     }
 };
}());

```


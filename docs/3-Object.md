# 第三章 物件

JavaScript 的簡單型別：數值、字串、boolean、null、undefined。其他所有值都是**物件**（object）。
簡單型別具有他門自己的方法，這些型別**永遠不會改變**，JavaScript 裡的物件，是可變異的定調集合。

JavaScript 的物件不受類別束縛，特性名稱或值都沒有限制，物件也能包含另一個物件。

JavaScript 包含原型聯繫（prototype linkage）功能，允許物件繼承其他物件的特性，這個功能可以減少物件初始化的間，也能減少記憶體的消耗。

## 3-1 物件實字

物件實字（object literal），建立新物件值得便利註記方式。
```sh
var empty_object = {};

var stooge = {
    "first-name" : "Joe",
    "last-name" : "Howard"

};
```
特性名稱可為任何字串，包括空字串，若名稱適合格的名稱且沒保留字，則可以選擇是否使用引號圍起。名稱/值以逗號隔開。可為巢狀構造。
```sh
var flight = {
    airline : "Oceanic",
    number : 815,
    departure {
         IATA : "SYD",
         time : "2015-09-23 10:42",
         city : "Sydney"
    },
    airline : {
         IATA : "LAX",
         time : "2015-10-23 09:42",
         city : "Los Angeles"
    }

};
```

## 3-2 擷取

用 [] 圍起一段字串運算式，即可從物件中擷取。如果運算式是個常數，且適合格的 Javascript 且非保留字，則可用 『. 註記』。
```sh
stooge["first-name"]    // "Joe"
flight.departure.IATA   // "SYD"

// 若為不存在的成員，則會產生 undefined 值。
stooge["middle-name"]    // undefined
flight.status            // undefined
```
|| 運算子能用於填入預設值
```sh
var middle = stooge["middle-name"] || "(none)";
var status = flight.status  || "unknown";
```
若從 undefined 擷取值，則會丟出 TypeError 例外，但可用 && 運算字防衛。
```sh
flight.equipment                             // undefined          
flight.equipment.model                       // 丟出 "TypeError"
flight.equipment && flight.equipment.model   // undefined 
```
## 3-3 更新

物件裡的值，可以透過指派而更新，若物件裡還沒有特性名稱，則會增加到物件裡。
```sh
stooge["first-name"] = "Shon";    //指派值
stooge.nickname = "Shon";
flight.status ＝'overdue';        //增加新的特性名稱flight.status

```

## 3-4 參考
物件透過**參考**（reference）而被四處傳遞。物件不會被複製：(範例是要接上的喔，否則一定會錯)
```sh
var x = stooge;
x.nickname = 'Shon';
var nick = stooge.nickname;

// x 和 stooge 參考相同的物件
// 所以 nick 是 'Shon'

```

## 3-5 原型
每個物件都聯繫到一個 prototype 物件，由此可繼承物件。底下例子將為 object 函式增加一個 beget 方法。
```sh
if (typeof object.beget !== 'function'){
   object.beget = function (o){
       var F = function () {};
       F.prototype = o;
       return new F();
   };
}

var another_stooge = object.beget(stooge);
```
對原型的聯繫，更新時沒有效用。當我們改變物件時，對物件的原型未受到碰觸：
```sh
another_stooge['first-name'] = 'Harry';
another_stooge['middle-name'] = 'Moses';
another_stooge.nickname = 'Moe';
```

原型聯繫只用於值的擷取，如果對物件擷取 prototype 值，而且物件缺少該特性，JavaScript 會嘗試**從原型物件**中擷取特性值，如果物件還是缺少該特性名稱，則會導向他的原型，**層層上推**，直到抵達 object.prototype。
若還是沒有則結果為 undefined 值，這段過程叫 **delegation**(委託)。

## 3-6 反映（反射）

嘗試擷取特性並檢查取得的值，就能檢視物件，同時判斷其中有那些特性。

  * typeof 判斷特性型別:
     ```sh
     typeof flight.number      // 'number'
     typeof flight.status      // 'string'
     typeof flight.arrival     // 'object' 
     typeof flight.manifest    // 'undefined'
     
     // 特別注意底下的值回傳是 functiuon 
     typeof flight.toString       // 'function'
     typeof flight.constructor    // 'function'

     ```
   * hasOwnProperty 擁有指定特性回傳，若是 則為 true，**不會檢索原型鍊**。
       ```sh
        flight.hasOwnProperty('number')      // true
        flight.hasOwnProperty('constructor') // false
       ```
## 3-7 列舉

for in 敘述可用迴圈處理所有物件的特性名稱，包括你沒有興趣的函式特性。且名稱順序沒有保證，如果想要保持順序請改用陣列的方式。

## 3-8 刪除

delete 運算字可從物件中移除特性，不會影響原型聯繫裡的其他物件。
```sh
another_stooge.nickname    // 'Moe'

//從 another_stooge 裡移除 nickname
delete another_stooge.nickname;

//顯示來自原型物件的 nickname
another_stooge.nickname    // 'Shon'

```      

## 3-9 減少全域變數
JavaScript 很輕易定義保存應用程式所有資產的全域變數。全域變數削弱的程式的彈性，應該避免使用。

  * 建立一個唯一一個全域變數
     ```sh
       var MYAPP = {};
       
       //然後用這個全域變數保存你的應用程式
       MYAPP.stooge = {
             "first-name" : "Joe",
             "last-name" : "Howard"
       }
     ```      
 後面會談到**閉包**（closure）技巧來有效的減少全域變數。     
       
# 第六章 陣列
陣列是記憶體的一種線性分配，其中的組件更據計算偏移量（整數）而存取。陣列可以式非常快速的資料結構。但可惜 JavaScript 沒有類似陣列的東西。
JavaScript 提供了類似陣列性格的物件。它把陣列註標（或稱索引index）轉換成字串。比實際的陣列慢，但更方便使用。

## 6-1 陣列實字
陣列實字是一對方括號圍起零到多個值，值以逗號分隔。
```sh
  var empty = [];
  var numbers [
      'zero','one','two'
      ];
  
  empty[1];   // undefined
  numbers[1];  // 'one'
  
  empty.length;  // 0
  numbers.length; // 3
```
下列為物件實字：
```sh
 var numbers_object = {
     '0' : 'zero',
     '1' : 'one',
     '2' : 'two'
 };
```
numbers 繼承自 Array.prototype ,而 numbers_object 卻繼承自 Object.prototype，所以 numbers 有較多的方法與神秘的 length 特性。但 numbers_object 則沒有。   
在多數的語言中，陣列元素必須為相同的型別， JavaScript 則允許陣列中包含各種型別的混合的值：
```sh
  var misc = [
      'string',98.6,true,null,undefined,
      ['neested','array'],{Object:true},NaN,
      Infinity
     ];
  misc.length;    // 10
```

## 6-2 length 特性
每一個陣列都有 length 特性，JavaScript 陣列的 length 並不代表陣列的上限，如果儲存的元素大於目前的 length 得註標，則 length 將增加以包含新的元素，不會出現已達陣列上限的錯誤。
**length 特性，是陣列中最大的整數特性名稱加 1 ** 不一定式陣列中的特定數量：
```sh
  var myArrat = [];
  myArrat.length;  // 0
  
  myArrat[100000]  = true;
  myArrat.length;  // 100001
  //myArrat 只包含一個特性
```
可以明確設定 length，設定的 length 比陣列元素多，並不會分配較多的空間給陣列，反之，則將被刪除。
```sh
  numbers.length = 2;
  // ['zero','one']
```
新的陣列元素，可藉由指派陣列目前的 length 而被附加至陣列尾端：
```sh
  numbers[numbers.length] = 'shi';
  // ['zero','one','shi']
  
  // 使用 push 方法更容易達到同樣效果
  numbers.push('go');
   // ['zero','one','shi','go']
```
## 6-3 刪除
既然 JavaScript 的陣列其實是物件，delete 運算子即可用於移除陣列裡的元素。
```sh
  delete numbers[2];
  // ['zero','one',undefined,'go']
  
```
這麼做將在陣列裡留下破洞。幸好 JavaScript 有個 splice 方法，它可以對陣列動手術，刪除某些元素，並以其他元素代替。   
[splice](http://www.w3schools.com/jsref/jsref_splice.asp) 第一個引數是陣列裡的序號，第二個引數是欲刪除的元素數量。
```sh
  numbers.splice(2,1);
 //  ['zero','one','go']
```
若是很大的陣列，這項工作可能要花很一點時間。
## 6-4 列舉
因為 JavaScript 的陣列其實式物件，即可使用 for in 敘述處裡陣列中所有特性。但 for in 無法保證順序，大多數陣列程式都期待產出順序排列的元素。還有從原型鍊裡挖出非預期性的問題。
方便好用的 for 敘述能避免上述問題。 for 敘述由三個子句控制：
 * 第一個子句初始化迴圈
 * 第二個子句是 while 條件據
 * 第三個子句負責遞增
 
 ```sh
 var i ;
 for (i = 0; i < myArrat.length; i += 1)
 {
   document.writeln(myArrat[i]);
 }
 ```
## 6-5 困惑之處
JavaScript 沒有更好的陣列與物件區分機制，可以透過自訂 is_array 函式，可繞過此向缺點：
```sh
 var is_array = function(value){
    return value && 
           typeof value === 'object' && 
           value.constructor === Array;
 };
```
很可惜，自訂函式無法辨識在不同視窗或框架下建構證列，需加點修改。
```sh
 var is_array = function(value){
    return value && 
           typeof value === 'object' &&
           typeof value.length === 'number' &&
           typeof value.length === 'function' &&
           !(value.propertyIsEnumerable('length'));
 };
 
```
首先，判斷使否為 true，這個動作用於拒絕 null 和其他視為 false 的值，第二個輪詢，是否為 object ，這個輪詢遇到物件、陣列和 null 都會是 true 。第三輪詢值是否有 length 特性，
且為數值，陣列本項必須為 true ，但物件通常不會。最後詢問 length 特性是否可列舉（length 特性是否由 for in 迴圈產生?）。陣列遇到此項檢測均為 false 。
## 6-6 方法
JavaScript 提供一組對陣列行動的方法，這些方法就是儲存在 Array.prototype 裡的函式。如同第三章 Object.prototype 如何擴充。假如我們要擴充 array 方法，對陣列做計算：
```sh
Array.method('reduce',function (f,value) {
     var i;
     for (i = 0; i < this.value.length; i += 1)
     {
        value = f(this[i],value);
     }
     return value;
});

```
對 Array.propotype 增加函式後，每個陣列都繼承了新增的方法。
```sh
 var data = [4,8,15,16,23,42];
 
 var add = function(a,b){
          return a + b;
 };
 
 var mult = function(a,b){
          return a * b;
 };
 
 // 呼叫擴充方法 reduce 傳入 add 函式
 var sum = data.reduce(add,0); // sum = 108
 
 // 呼叫擴充方法 reduce 傳入 mult 函式
 var sum = data.reduce(mult,1); // sum = 7418880
   
```
因為陣列事實上是個物件，可以把方法加入單一陣列：
```sh
 data.total = function () {
      return this.reduce(add,0);
 };
 
 total = data.total();  // 108 
```
既然字串 'total' 不是個整數，對陣列新增 total 特性不會改變他的 length 。由此產生的物件，將繼承陣列的值於方法，但部會擁有 length 特性。
## 6-7 陣列維度
JavaScript 陣列通常不會初始化，如果你用 [] 要求新陣列，它將是空的，如果取用不存在的元素，將會獲得 undefined 值。擴充修正這項疏忽：
```sh
 Array.dim = function (dimension, initial){
     var a = [],i;
     for (i = 0; i < dimension; i += 1){
        a[i] = initial;
     }
     return a;
 }
 
 // 製作包含十個 0 的陣列
 var myArrat = Array.dim(10,0);
 
```
想製作二維陣列，或陣列中的陣列時，必須自己建立，我們可以擴充：
```sh
 Array.matrix = function (m,n, initial){
     var a, i, j,mat = [];
     for (i = 0; i < m; i += 1){
        a = [];
        for (j = 0; j < n; j += 1){
          a[j] = 0;
        }
        mat[i= a;
     }
     return mat;
 }
 // 製作一個 4 * 4 填滿 0 的矩陣
 var myMatrix = Array.matrix(4,4,0);
 document.writeln(myMatrix[3][3]);  // 0
```
製作完全相同矩陣方法
```sh
 Array.identity = function (n){
    var i , mat = Array.matrix(n,n,0);
     for (i = 0; i < n; i += 1){
        mat[i][i] = 1;
     }
     return mat;
 };
 
 myMatrix = Array.identity(4);
 document.writeln(myMatrix[3][3]);  // 1
```
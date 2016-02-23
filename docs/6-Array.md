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
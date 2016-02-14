# 第五章 繼承
在類別程式語言中（如 Java），繼承提供兩項好用的服務。
  * 原始碼的重複再利用：新的類別大致相同，只須設計不同的部份
  * 包含型別系統的詳細規則：幾乎不需要撰寫明確的型別轉換
  
JavaScript 作為一個寬鬆的語言，沒有型別的轉換，物件的世家在此無關緊要，重點在於物件的能耐，而不在於物件有哪些前輩。
JavaScript 可行的繼承模式非常廣泛，我們將討論最簡單的幾種，另外雖有更複雜的結構，但通常最好保持模式的簡單。
JavaScript 是一種原型語言，他的物件直接繼承其他物件。

## 擬類別繼承模式
JavaScript 並非直接讓物件繼承物件，而是插入一層不必要的間接性。使物件由建構式（constructor）產生。  
建立物件時，產生函式物件的 Function 建構式，執行類似下列原始碼：
```sh
 this.prototype ={constructor: this};
```  
新的物件被給予 prototype 特性，其特性值為包含 consrtuctor 特性的物件，而這個特性的值則是新的函式物件。
 prototype 物件是存放繼承特質的地方，每個函式會拿到一個 prototype 物件，因為 JavaScript 並未提供判斷用於建構式的函式途徑。constructor 特性沒什麼用，**真正的重點在 prototype 物件**。
 
當函式使用字首詞 new 、以建構式呼叫模式被呼叫時，即可調整執行函式的方法。
```sh
 Function.method('new', function(){
   // 建立新物件，繼承自建構式的 prototype
   var that = object.beget(this.prototype);
   
   //呼叫建構式 繫結 this 至新物件
   var other = this.apply(that, arguments);
   
   //如果它回傳值並非物件，則替代新的物件。
   
   return (typeof other === 'object' && other) || that;
 });
```
我們可以定義建構式，以及擴展他的 prototype :

```sh 
 var Mammal = function (name){
     this.name = name;
 };
 
 Mammal.prototype.get_name = function () {
     return this.name;
 };
 
 Mammal.prototype.says = function (){
     return this.saying || '';
 };
 
 //實做
 var myMammal = new Mammal('Hreb the Mammal');
 var name = myMammal.get_name();  // 'Hreb the Mammal' 
```
我們可以製作另一個繼承 Mammal 的擬類別，透過定義擬類別的 constructor 函式，以及用 Mammal 的實例去代他的 prorotype。
```sh
 var Cat = function (name){
     this.name = name;
     this.saying = 'meow';
 };
 
 // 以 Mammal 的新實例取代 Cat.prototype
 
 Cat.prototype.purr = function (n){
     var i, s ='';
     for (i = 0; i < n, i +=1){
         if (s){
            s += '-';
         }
         s += 'r';
     }
     return s;
 };
 
 cat.prorotype.get_name = function (){
     return this.says() + ' ' + this.name + ' ' + this.says();  
 };
```





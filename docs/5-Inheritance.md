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
 
 var myCat = new Cat('Henrietta');  
 var says = myCat.says(); // 'meow'
 var purr = myCat.purr(5);  // 'r-r-r-r-r'
 var name = myCat.get_name(); // 'meow Henrietta meow'
```
原本希望擬類別模式能讓 JavaScript 看起來像物件導向，實際上卻怎麼看怎麼怪。透過使用 method 方法，並定義 inherites 方法，可隱藏部份的醜陋。

```sh
Function.method('inherites',function (Parent){
     this.prototype = new Parent();
     return this;
});

```
現在可以只用一行敘述，製造我們的 Cat :
```sh
 var car = function (name){
     this.name = name;
     this.saying = 'meow';
 }.
 inherites(Mammal).
 method('purr',function (m){
    var i, s ='';
    for (i = 0 ;i < n; i += i){
       if (s){
           s += '-';
       }
       s += 'r';
    }
      return s;
 }).
 method('get_name',function (){
      return this.says() + ' ' + this.name + ' ' + this.says(); 
 });
```

藉由隱藏 prototype 原始碼看起來沒那麼詭異了。這裡沒有 privacy，所有的特性都是 public 。沒有取用 super 方法的途徑。   
更糟糕的是，使用建構式，如果沒有加上字首詞 new , this 不會繫結至新物件，this 將會繫結全域變數，不會有編譯警告，也不會有執行錯誤警告。   
為了緩解此問題，習慣把所有建構式以首字母大寫的格式命名，此外沒有其他的東西的名稱採取首字母大寫的格式，希望有了視覺上的審查，可以找出缺少的 new 。 
 
## 物件規格器繼承模式
有時候，建構式被賦予非常多的參數，這些引數的順序將不太容易記得，這時可以撰寫**單一物件規格器**（object sperifier）的建構式將較為友善。   
原本：
```sh
var myObject = maker(f,1,m,c,s);
```
改寫成：
```sh
  var myObject = maker ({
      first : f,
      last : 1,
      state : s,
      city : c
  });
```
引數現在能以任何順序列出，原始碼也容易理解。如果建構式接受物件規格，就可以簡單的把 JSON 物件傳送給建構式，而後將會回傳完整組成的物件。

## 原型繼承模式
一個新物件能繼承現有物件的特性。剛開始製作一個有用的物件，然後就能製作更多像它的物件，如此可以避免『把一個應用程式分解為巢狀抽象類別』的分類過程。
```sh
//使用物件實字，製作有用的物件開始
var myMammal = {
    name = 'Herb the Mammal',
    get_name : function(){
         return this.name;
    },
    says : function(){
         return this.saying || '';
    }
};
```
有了理想的物件後，即可使用第三章的 object.beget 方法製作更多實例。
```sh
  var myCat = object.beget(myMammal);
  myCat.name = 'Henrietta';
  myCat.saying = 'meow';
  myCat.purr = function (n){
      var i, s ='';
      for (i = 0 ;i < n; i += i){
        if (s){
            s += '-';
        }
         s += 'r';
        }
      return s;
  };
  
  myCat.get_name = function (){
     return this.says() + ' ' + this.name + ' ' + this.says(); 
  };
```
這是**差別繼承**(defferential inheritance)。自訂新物件時，指出它與根基舊物件的差別。

## 函式繼承模式
目前我們看到的繼承模式，有個缺乏隱私（privacy）的缺點，物件中所有的特性都能看得到。我們沒有 private 變數或方法。   
從物件一個物件，首字母不要大寫，函式中包括四個步驟：
 * 建立新物件
 * 選擇性的定義 private 實例變數與方法。函式只有傳統的 var
 * 以上述步驟為物件擴充方法。這些方法對參數和 var 定義的事物有優先存取權。
 * 回傳新物件。
 
下列為函式的建構式虛擬樣板
```sh
var constructor = function (spec,my){
    //other provate instance variables
    var that, my = my || {};
    
    //Add shared variables and functions to my
    
    that = a new object;
    
    //add privileged method to that
     
    return that; 
}
```
spec 物件包含所以有建構式製作實例所需的資訊，spec 的內容可被複製到 private 變數中，或被其他函式轉換。或者，方法也能在需要時存取 spec 物件的資訊。（簡化方式則是用單一直取代 spec 當建構的物件不需要整個 spec 物件，簡化方式就很有用。）   
my 物件是祕密資訊容器，祕密資訊由繼承鍊上的建構式共享。my 物件為選用，如果未傳送 my 物件，則製作一個 my 物件。   

接下來宣告物件的 private 實例變數與 private 方法：**只須宣告變數即完成**。建構式的變數和內層函式，變成實例的 private 成員。內層函式對 spec、my、that，以及 private 變數具有存取權。   
然後加入共享木密資訊到 my 物件中。
```sh
 my.member = value;
```
現在，製作了一個新物件，並至派給 that。製作新物件有很多方式。可以使用物件實字;可以呼叫利用 new 運算子的擬類別建構式;可以在原型物件上使用 Object.beget 方法。或者我們可以呼叫另一個建構式，傳給它一個 spec 物件和 my 物件。   
下一步是擴展 that，加入製作物件介面的優先方法。我們可以指派新的函式給 that 的成員。或採用更安全的方式，先把函式定義為 private 方法，在指派給 that:
```sh
vat methodical = function (){
     .......
};
that.methodical = methodical;
```
使用兩個步驟定義 methodical 的好處，在於其他方法若想呼叫，可直接呼叫 methodical()，而不是 that.methodical()。如果實例損壞或竄改了被取代的 that.methodical，呼叫 methodical 的方法仍可同樣運作。   
最後回傳 that 。

把這種模式套用到 mammal 範例中，此時不需要 my，單純省略它。name 與 saying 特性，現在都完全的 private，只能透過特許的 get_name 與 says 方法取用。 
```sh
var mammal = function (spec){
    var that = {};
    
    that.get_name = function (){
         return spec.name;
    };
    
    that.says = function () {
         return spec.saying || '';
    };
    
    return that;
};

var myMammal = mammal({name:'Herb'});
```
在擬類別模式裡，Cat 建構式必須複製 Mammal 建構式已經完成的工作。但在函式模式裡，則不需要這樣做。**因為 Cat 建構式將呼叫 Mammal 建構式，讓後者負責大部分物件建設工作，Cat 則只需關心有差異的地方**。
```sh
 var cat = function (spec){
     spec.saying = spec.saying || 'meow';
     vat that = mammal(spec);
     that.purr = function (n){
        var i, s ='';
      for (i = 0 ;i < n; i += i){
        if (s){
            s += '-';
        }
         s += 'r';
        }
      return s;
     };
     that.get_name = function (){
         return this.says() + ' ' + this.name + ' ' + this.says(); 
     };
     return that;
 };
 
 var myCat = cat({name:'Henrietta'});
```
函式模式也給我們處裡 super 方法的方式。我們將製作一個 superior 方法，**接收方法名稱，並回傳呼叫該方法的函式**。即使改變特性，函式也將呼叫原始方法：
```sh
  Object.method('superior',function (name){
     var that = this,
         method = that[name];
     return function () {
         return method.apply(that, arguments);
     };    
  });
```
我們用 coolcat 試試看，它就像 cat ，只不過比較酷、呼叫 super 方法的 get_name 方法。我們將宣告一個 super_get_name 變數，並呼叫 superior 方法的結果指派給它：
```sh
 var coolcat = function (spec){
     var that = cat(spec),
         super_get_name = that.superior('get_name');
     that.get_name = function (n){
         return 'like ' + super_get_name () + ' baby';
     };    
     
     return that;
 };
 var myCoolCat = coolcat({name : 'Bix'});
 var nemt = myCoolCat.get_name(); // 'like meow Bix meow baby'
 
```
函式模式具有最佳彈性，它比擬類別模式需要的工作量更少，而且給我們更好的概括與資訊隱藏，以及對 super 方法的存取。

如果物件的所有狀態都是 private ，這個物件就可以防止損害。物件的特性能被取代或刪除，但物件的整體性不受妥協。如果我們用函式風格建立物件，而且所有物件裡的方法都不使用this 或 that ，則該物件為**持久的**（durable）。一個持久耐用的物件，是一組函式的集合，其函式的行為像是 capablity 。

## 零件繼承模式
用一組零件，可以組合出物件。例如說，我們可以為任何物件製作一個加入簡易事件處裡功能的函式。它會增加一個 on 方法、一個 fire 方法，還有一個 private 事件登記處：
```sh
  var enentuality = function (that){
      vat registry = {};
      that.fire = function (event){
        // 對物件發出事件，事件是可以包含事件名稱的字串，或是一個物件，
        // 該物件裡包含的 type 特性，保存了事件名稱。
        // 處裡器由 on 方法登記，
        // 其中符合事件名稱的處裡器將被呼叫
        
        var array,
            func,
            handler,
            i,
            type = typeof event === 'string' ? event : event.type; 
      
      // 如果事件有個處裡器陣列，則以迴圈方式處裡陣列，並依序執行處裡器。
      
         if (registry.hasOwnPreoperty(type)){
             array = registry[type];
             for (i = 0; i < array.length; i += 1){
                  handler = array[i];
       // 處裡器的紀錄，包含一個方法與一個選用參數陣列，
       // 如果方法是個名稱，則尋找符合該名稱的函式           
                  func = handler.hethod;
                  if (typeof func === 'string'){
                     func = this[func];
                  }
       // 呼叫處裡器。如果紀錄包含參數，則傳遞參數，否則傳遞 event 物件。
                  func.apply(this,handler.paramerers || [event]);           
                  
             }
         }
        return this;
      };
      that.on = function (type,method,paramerers){
        // 登記一個事件（event）。製作處裡器紀錄。
        // 放入處裡器陣列，如果類型事件尚未存在，則製造一個。
        var handler = {
            method : method,
            paramerers : paramerers
        };
        if (registry.hasOwnPreoperty(type)){
            registry[type].push(handler);
        } else {
            registry[type] = [handler];
        } 
        return this;
      };
      return that;
  };
```
我們可以在任何物件裡呼叫 eventuality，給它事件處裡的方法。我們也可能回傳 that 前，在建構式裡呼叫 eventuality(that) 。如此一來，建構式可從一組零件組合出物件。JavaScript 的寬鬆型別在此大有益處，因為不受型別系統的拖累，我們可以專心處裡內容。   
如果我們想讓 eventuality 取用物件的 private 狀態，則可以把 my bundle 傳過去。






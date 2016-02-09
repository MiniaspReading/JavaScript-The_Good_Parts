# 第二章 語法
本章介紹 JavaScript 優良部分的文法，概述語言的結構方式。

## 空格

* 有時必須使用空格區分一連串字元，以免被合成單一 token(字組)。
  ```sh
  var that = this; 
  ```
* JavaScript 提供兩種註解形式：   
  區塊註解形式: \/\* 與 \*\/  
  單行註解形式: \/\/ 起始
  使用區塊註解暫時關閉一塊原始碼，不見得安全。
  ```sh
  /*
    var rm_a = /a*/.match(s);
  */
  ```
  上例將造成語法錯誤，此時建議改用 \/\/ 單行註解方式。
  
## 名稱
  
名稱至少有一個文字字母，其後可以接續一到多個文字、數字或底線。但名稱不可為保留字。
> ECMA-262 第三版中保留字的完整列表如下：   
abstract   
boolean   
byte   
char   
class   
const   
debugger   
double   
enum   
export   
extends   
final   
float   
goto   
implements   
import   
int   
interface   
long   
native   
package   
private   
protected   
public   
short   
static   
super   
synchronized   
throws   
transient   
volatile   

還有包括 undefined、Nan、Infinity 等等的保留字。此外**變數**或**參數**不許以保留字命名。保留字也不可作為**物件實字的物件特性的名稱**，也不可接在點號後。

**名稱**用於敘述(statement)、變數（variable）、參數（parameter）、特性名稱(property name)、運算子(operator)和標籤（label）。

## 數值

**JavaScript 只有一個數值(number)型別**。數值型別表現為64位元的浮點數，無單獨的整數型別，所以 1 與 0.1 是相同的值。

數值為指數時，則把 e 前的部份乘上10的次方，次方由e後面的部份決定，如：
  100 等於 1e2 。

負數可在字首加上 - 運算子構成。

NaN 屬於數值，他代表『**運算無法產生正常結果**』，他不等於任何值，包括 NaN 自己。可使用 isNaN(number) 函式偵測 NaN 。

Infinity 值代表大於 1.79769313.....e+308 的所有值。

數值有自己的方法，後面會提到....待續。


## 字串

字串可用單引號（'）或雙引號（"）圍起。

\\(反斜線)則作為轉義（跳脫）字元，所有字元都屬16位元長度。  
   **轉義字元**
   * \\"  雙引號
   * \\'  單引號
   * \\\  反斜線
   * \\/  斜線
   * \\b  倒退
   * \\f  換頁
   * \\n  換行
   * \\r  游標歸位
   * \\t  tab
   * \\u  4個十六進位數

JavaScript 沒有 character 型別，若要表示字元，請維持字串裡只有一個字元即可。

字串下有 length 特性   
ex: "seven".lngth 為 5

字串永不改變，可使用 "+" 運算子串連其他字串，將被視為相同字串。   
ex: 'c'+'a'+'t' === 'cat'  \/\/結果為 true

## 敘述

  **var敘述**
  * var 敘述可以定義函式的私有變數。
  
   **中斷敘述**
  * break
  * return
  * throw
  
  **區塊**   
  用一組大括號{}圍起來的敘述，區塊不會建立新的**範圍（scope）**，所以變數應該定義在函式的頂端。
  
  **if 敘述**  
  更據運算是的值改變程式的流向。
  
  false 家族：
  * false
  * null
  * undefined
  * 空字串 ''
  * 數值 0
  * 數值 NaN
  
  所有其他值都被視為 true ，包括 true 本身、字串'false' 和所有物件。
  
   **switch 敘述**  
   必須指定 case，會與所有的 case 比較尋找相等的 case。若無找到，則執行選用的default 敘述。
   case 最後的敘述必須包含中斷，以免執行到下一個 case。  
   [語法參考](http://www.w3schools.com/js/js_switch.asp)
   
   **while 敘述**  
   如果運算式為 false 迴圈即中斷。  
   [語法參考](http://www.w3schools.com/js/js_loop_while.asp)
  
   **for 敘述**  
   透過三個選用子句而控制:
     * 初始句：通常為迴圈的初始變數
     * 條件句：是否達到完成標準，若省略則預設估算結果為 true
     * 遞增句：遞增或遞減變數後，在執行條件句開始重複
   
   另一種形式 （for in） 列舉物件的特性，每輪迴圈均把物件的另一個特性名稱字串，指派給變數。
   ```sh
   for (myvar in obj){
       if (obj.hasownProperty(myvar)){
          ....
       }
   }
   ```
   [語法參考](http://www.w3schools.com/js/js_loop_for.asp)
   
   **do 敘述**  
   區塊一定會至少執行一次。   
   [語法參考](http://www.w3schools.com/js/js_loop_while.asp)
   
   **try 敘述** 
   try 執行一個區塊，並捕捉任何區塊丟出的例外狀況，catch 子句定義了接收例外的變數。
   ```sh
     try {
       adddlert("Welcome guest!");
      }
     catch(err) {
       document.getElementById("demo").innerHTML = err.message;
      }
   ```
   [語法參考](http://www.w3schools.com/js/js_errors.asp)


   **throw 敘述**    
   throw 敘述負責發出例外狀況，若 throw 在 try 區塊裡，控制權會交給 catch 子句。
   
   [語法參考](http://www.w3schools.com/js/js_errors.asp)
   
   
   **return 敘述**    
   return 敘述使得函式提早回傳，也可以指定回傳值，若未指定 return 裡的運算式，回傳值為 undefined。
   
   [語法參考](http://www.w3schools.com/jsref/jsref_return.asp)
   
   **break 敘述**    
   能跳出迴圈或 switch 敘述。
   
   [語法參考](http://www.w3schools.com/js/js_break.asp)
   
   
## 運算式
最簡單的運算式：實字值（一個字串或數字）、內建值（true、false、null、undefined、NaN、Infinity
）、new 後接呼叫運算式、delete 後接精確運算式、以（）圍起的運算式、皆有字首詞運算子的運算式。
  * 嵌入式運算子和其他運算式
  * ? 三元運算子後接另一各運算式，： 之後再接一個運算式
  * 一個呼叫式
  * 一個精確式

參考 [運算子優先順序](https://msdn.microsoft.com/zh-tw/library/z3ks45k7%28v=vs.94%29.aspx)

  **字首運算子**
  * typeof():type of
  * +：數值
  * -：負號
  * !：邏輯 Not
    
  **嵌入式運算子**
  * +：能做數值的加法或字串的相連
  * *：乘
  * /：除
  * %：取餘數
  * ===：完全相等
  * !=：不等於
  * ||：邏輯 OR 如果第一各運算元為 true，會產生第一各運算元的值，反之為第二各運算元的值
  * &&：邏輯 AND，如果第一各運算元為 false，會產生第一各運算元的值，反之為第二各運算元的值
  
  **精確式**
   * .[名稱]：指定物件特性
   * [運算式]：陣列元素
  
## 實字
後面討論..
## 函式
後面討論..

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

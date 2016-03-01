# 第七章  正規運算式 
JavaScript 有許多其他語言借用的功能。他的語法來自 Java、函式來自 Scheme、原型式繼承來自 self、正規運算則來自**Perl**。   
JavaScript 與正規式合力的工作方法有：
 * regexp.exec()
 * regexp.test()
 * string.match()
 * string.replace()
 * string.search()
 * string.split()

上列正規式通常具有顯著的效能優勢。

**請參考書上範例，此處不寫**
**但我參考『JavaScript 大全』第十章的正則表達式，更容易看懂。**

正規式在短小而簡單的狀況下運作最好，只有如此，才能自信的確認正規是將正確運作，且能在需要時成功修改。   
在 JavaScript 語言處裡器間，有非常高度的兼容性，其中可攜性（portable）最底部份，就是正規式的實作。巢狀正規式也可能在某些實作中出現糟糕的效能問題。**簡單**，是最好的策略。
## 7-1 建構

 


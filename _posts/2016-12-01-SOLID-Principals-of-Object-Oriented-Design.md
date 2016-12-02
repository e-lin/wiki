---
layout: post
title:  "The SOLID Principals of Object Oriented Design"
date:   2016-12-01 00:00:00 +0900
categories: jekyll update
type: Algorithms/Design
excerpt_separator: <!--more-->
---
設計模式的六大原則
把握六大原則可達到易維護, 易擴充, 可讀性高, 複雜度降低...等等好處
<!--more-->


Single Responsibility Principle
---

> Single responsibility to each Object.

**1. 單一職責**

類的職責要單一, 如下例子: 將原本維護用戶信息的類圖, 畫分成負責用戶屬性以及負責用戶行為的類圖
![Imgur](http://i.imgur.com/519k9tp.jpg)


Liskov Substitution Principal
---

> Base Class and Derived class follow ‘IS A’ principal

**2. 里氏替換**

父類出現的地方可替換為子類:

- 子類必須完全實現父類的方法
- 子類可以有自己的個性 (子類出現的地方不可替換為父類)
- 覆蓋或實現父類的方法時輸入參數可以被放大 (子類方法中的參數必須與超類中被覆寫的方法的參數相同或者更寬鬆)
- 覆寫或實現父類方法時輸出結果可以被縮小

如下例子: 手槍、步槍、機關槍都是槍, 而且可以射擊; 但玩具槍就不是真實的槍, 不能射擊, 不可為槍的子類
![Imgur](http://i.imgur.com/TQ3ktsj.jpg)


Dependency Inversion principle
---

> Reduce the dependency In composition of objects.

**3. 依賴倒置**

- 高層模塊不依賴低層膜塊, 兩者都該依賴各自的抽象
- 抽象不依賴細節
- 細節依賴抽象

如下例: 司機不一定都是張三, 開的車也不一定是賓士, 抽象出IDriver跟ICar
![Imgur](http://i.imgur.com/Tk1yRfa.jpg)


Interface segregation principle
---

> If an implementation don’t require then don’t implement it.

**4. 接口隔離**

接口不要太過肥大, 盡量對接口拆分
如下例: 美女的條件是長相好、身材好、有氣質; 但生在楊貴妃年代, 就不能實做niceFigure這個函式了, 那麼我們把原來的接口拆分成兩個 - 長相跟身材一個, 氣質一個
![Imgur](http://i.imgur.com/951yLbi.jpg)


Law of Demeter
---

> Least Knowledge Principle.

**5. 迪米特原則**

知道越少原則, 一個類只和朋友交流, 不與陌生類交流, 不要出現getA().getB().getC().getD()這種情況
如下例: 老師要求班長清點女生人數, 原本透過 teacher.command(new GroupLeader()) 達成, teacher.command 將初始化listGirls, 但是, 老師類並不需要與女生類有交流; 所以應該在班長類定義構造函數來傳遞女生類的依賴關係, 初始化listGirls移到main(), 透過 teacher.command(new GroupLeader(listGirls)) 來達成這個目的
![Imgur](http://i.imgur.com/pvBY7L8.jpg)


Open/Closed Principal
---

> On production level Objects are ready for extension for not for modification

**6. 開閉原則**

軟件應該通過擴展來實現變化, 而不是通過修改已有的代碼來實現變化(Be opened for extension, but closed for modification)


A Recap
---

1. is talking about your **class**:<br />
make your class simple, with a single responsibility.

2. is talking about the **class and subclass**:<br />
your subclass can always be replaced by the class. you can always overrite with more parameters to the method of the class.

3. is dependecy of **class and interface**:<br />
a driver drive a car, you do not need to specify the driver as whom, or to specify the car as BMW or something.


4. is about the **interface**:<br />
make your interface simple, with a single responsibility.


5. is saying **extension instead of modification**:<br />
your objects should be open to extend, close to modify.


6. The last one is not in the SOLID priciple. It’s talking about **Least Knownlege Principle**, saying that if A asks B to access C. Only B has necessary to know about C. A does not. 


Reference
---
- 設計模式之禪
- [六大軟體開發基礎設計原則][R1]
- [Object Oriented Design Interview For Parking Lot Or Elevator][R2]

[R1]: http://ithelp.ithome.com.tw/question/10095511?tag=rt.rq
[R2]: http://www.hiringlibrary.com/object-oriented-design-interview-for-parking-lot-or-elevator/


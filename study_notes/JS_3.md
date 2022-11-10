###### tags: `JavaScript` `this`

# 2022/10/27 JavaScript-物件導向 第6堂


## addEventlistner的 "click"事件 運作方式

**Capturing → Targeting → Bubbling**
1. target ： 事件流最後Ｕturn 回來碰到的對象
 
2. currenttarget : 事件流註冊的對象就是currenttarget


![](https://i.imgur.com/gzrsJyh.png)

預設事件發生在冒泡階段(Bubbling)，先回傳B再回傳A；
```
addEventlistner("click",()=>{},false)
```
如果改成true，會在捕獲階段(Capturing)就執行，先回傳A再回傳B。
```
 addEventlistner("click",()=>{},true)
```
一次只能選一條路走


若是想讓事件停止傳播 可使用StopPropagation


＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝

## Scope(可視範圍): 注意var、let、const的scope範圍
1.Scope chain(範圍鏈): 當前範圍找不到就會往外面找，只有JavaScript、Python才有

2.lexical scope(詞彙範圍)：scope跟它寫在「哪裡」有關！！！
＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝
**每一個物件都有一種屬性__proto__**
 **如果沒有我們要找的屬性，先從自己開始找，然後從自己的proto往其他的物件尋找。直到找到有為止 or 沒有找到=>(回傳undifined)**

**每個function都有prototype屬性，除了箭頭函式 =>{ }。**
且prototype預設是空白物件{ }

❋ 另外需要注意ASI = Auto Semicolon Insertion，自動加上分號;
這個機制，
如果return (
    要回傳的內容
)。
這樣就可以正確執行。return 沒有用()的話，會出現錯誤。

[lydia小測驗 關鍵字:prototype](https://github.com/lydiahallie/javascript-questions)
＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝
## this 百變怪（歡迎大家踴躍矯正，補充舉個例＾＾）
1. 誰呼叫就是指向誰
2. 如果自己沒人呼叫他，就是自己本身，全域物件，沒有呼叫主詞都是全域。
3. 箭頭函示沒有自己得this，所以自己會指向全域物件
4. 新增物件是否有用new，有new會轉向當前function裡的this=>{}，若函數本身若不用new丟入引數，會使this會指向全域，呼叫函數會undefine
5. 用call,apply,bind會強迫去取分，也就是會強迫引用當前輸入數值，並不會指向呼叫本人內的數值

##  array.prototype.funtion()
1. 函數是物件的一等公民，他有物件__proto__屬性和本身的prototype屬性，
2. 所有物件都有__proto__ 屬性會把相關物件串起來，形成鏈的連結，順著源頭一層一層往裡找
3. new會把後方函數的串連起來，通過prototype會連結至函數本身，呼叫的原型this去建立，而this此時會指向被呼叫原型的函數本身




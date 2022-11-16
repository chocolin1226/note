###### tags: `Ruby & Rails`
# 20221108 Ruby on Rails tips 

## 走出New Rails專案的第一步
1. rails專案的時候可以先用`rails/ruby -v` 確認版本
2. `rails / ruby 1.1.1--default` 轉成預設值
3. `rails _6.1.7_ new 專案名稱`
4. `git init` 初始化,把git加入資裡夾
5. `git add .` 將file轉到暫存區.的意思是全部的意思,除非知道裡面有什麼檔案,不然建議不要用,還是以後面加檔案名稱.副檔名為主
7. `git commit -m "儲存名稱"`  將告一段落的東西或是今天不會再碰到的東西存入版控


---
## bundle install  && gemfile 的秘密 

1. `bundle install` ：是找到 `Gemfile` 之後去執行 `gem install..`並在系統中安裝套件,然後會產生 `Gemfile.lock`檔`bundle install`想解決得問題是版本問題可能會同時有很多檔案共用一個檔案,但每個需求的版本都不太一樣所以就會藉由`bundle install`去分版本給每給檔案需要的

## gemfile 的存在意義

2. `gemfile` 裡面是所有專案要用的套件,包在gurp裡面就不會直接安裝而是當要測試的時候才會安裝,但要是放在全域就在執行`bundle install`的時候就會全部被安裝


---
## 一些小技巧

1. Ruby ~> 1(Major).0(Minor).1(Patch) ~>(不會跳版號) 然後為什麼要用是因為可以保持版號更新跟維持相容性

2. `Ruby "1.0.1"` 直接寫死板號不會錯

## 建立完 Rails 專案後每個檔案到底在做什麼呢？ 

3. `ruby-version` 裡面是放專案的版本

![](https://i.imgur.com/rROYKGq.png)

4. Gemfile 裡面是專案內用的所有套件軟體

![](https://i.imgur.com/iR966p7.png)



---
## Ruby for Rails
* gem 是什麼？ `gem` 是 `ruby` 套件安裝管理工具
* JS 中是用 yarn 跟 npm install 並產生 node_modules檔案

* `gem env` -> 可以看到 `gem` 相關環境設定 
* ruby 套件是安裝在系統中，不是在專案當中。

* `bundle install`： 根據`Gemfile`透過`gem install`去下載安裝套件，同樣也是安裝在系統當中。產生`Gemfile.lock`。
* `Gemfile.lock`是為了解決套件工具版本相依性問題。
* `gem install`：
* REPL = Read-Eval-Print Loop 可以在不同的環境底下就能編輯code。
* 多行註解
```ruby
 =begin 
  中間是內容
  中間是內容
  中間是內容
 =end
```
* Ruby 命名慣例，蛇式命名法。 Ex: user_name
* javascript 命名慣例，駝峰式命名法。 Ex: UserName
* 多從指定： a, b = b, a ; 達到 a,b 互換的效果
* 大寫字母開頭就是常數。 ex: A = 1
* [冷知識] Ruby 的常數是可以被 re-assign
![](https://i.imgur.com/PsYA2fm.png)
* 單引號('')(%q)、雙引號("")(%Q)
* Ruby使用 ＃{} / JavaScript使用 ${}
* Ruby的世界只有 `nil`、`false` 是 false，其他都是 true
* 一個等號=，兩個等號==，三個等號===
### Exception(例外)
每個程式語言對於 exception 處理方式不同。
Ruby的世界，會有這樣的設計，可以讓function單純的處理 input 與 output 的關係。
單獨把判斷的事情拉出來做處理。
```ruby
begin
    可能出現例外的程式碼
rescue
    沒有指定Exception的類別的話，那就是出現錯誤就執行
end
```

---

## REPL 到底是什麼？

* **REPL = Read-Eval-PrintLoop** 想要解決不用任何檔案就可以做測試為了方便每個測試者更方便處理

## Ruby的命名法與值得互換
* Ruby 用**蛇式命名法** , JS 用**駝峰式命名法**

* a , b = b , a 兩值互換可以直接使用這方法 

## 常數及變的差異性,Ruby const can re-assign ?

* Ruby 常數是可以 re-assign ,但是會出現警告 , 另外一個差異性就是大小寫不同

![](https://i.imgur.com/V7jJmtG.png)

## 引號如何帶入參數,單引號及雙引號差別

* 引號的使用方式,要帶入參數要使用雙引號,單引號不會有作用

![](https://i.imgur.com/Tn4ULTC.png)

## 在Ruby的世界裡 === 不是比較而是一個 method
*  `kitty === 1` 不能把等於看作是比較而是比較像 `kitty.===(1)`在物件化的世界裡面 === 是一個方法並帶給他參數(1)

## 能不寫else就盡量不要寫
*   在if...else 內能不寫else 就盡量不寫反而比較直覺,可讀性也會比較好,如果真的要寫else 也可以用把原本相同的東西拉出來做成預設值,原本else的部分,就改用if判斷就可以,以下是簡化過後的 method 相對簡單很多  

```
def is_integer_than_20(n)

  if n > 20         return if > 20 
    return true     >>>這邊可以直接簡化成一行只要關注要關注的
  else
    return false
  end
end
```
## Exception 例外
使用`begin...rescue` ,讓 method 只用在意輸入值或輸出值就好越單純越好只用在意計算 , 不要讓 method 再去做判定！ 


## 今日課堂小練習
1. 使用map方法就是將陣列的每一個元素做運算後再回傳到新的陣列中。
```ruby
list = [1,2,3,4,5]
p list.map { |n| n * 2 }
# 有一個陣列[1,2,3,4,5] // 印出結果[2,4,6,8,10] 
```
2. reduce不給預設值就會,抓索引值[1],acc = 9 , num = 1 如果 9 > 1 就會是true就會留下 9 然後 num = 2 在一路做比較下去直到沒有參數就會回傳最終結果
```ruby
#使用 reduce 找出 list 的最大值   // 9

list = [9, 1, 2, 7, 3, 4, 5, 8]

p list.reduce{|acc , num| acc > num ? acc : num;} 
```
3. 先用`compact`把`array`中的`nil`先處理掉，接著使用`sort`讓陣列中的元素進行排序由小到大，最後使用`uniq`將陣列中重覆的元素過濾掉。
```ruby
#把陣列 [1, 3, 4, 1, 7, nil, 7] 由小到大排序，並且移除 nil 以及重複的數字
list = [1, 3, 4, 1, 7, nil, 7]

p list.compact.sort.uniq

```






## 個方法使用實戰大全
* compact → new_arrayclick to toggle source
Returns a new Array containing all non-nil elements from self:
compact > 回傳一個新的陣列

![](https://i.imgur.com/B916d1X.png)











---

### 為什麼使用變數？
賦予值意義。

### REPL?為何使用？
即時讀寫無窮迴圈，方便測試一些短方法。

### 常數與變數有何不同？為何如此設計？
常數大寫開頭。
其他語言常數不可改，ruby可以改，只是會跳警告。

因為ruby的類別是用常數命名，
類別會需要擴充功能，
如果常數不能改就無法擴充。

### if倒裝？使用時機？相反詞？慣用寫法？
```
puts "在家" if weather == "下雨" 
```

**使用時機：**
如果可以一句寫完，
或為了強調結果。

**相反詞：**（不推薦使用）
`unless` 或是 `if not`

**慣用寫法：**
1. 正向表述為主
2. 盡量不用寫`else`
3. 注意可讀性


### 例外處理？
* 讓方法專心在輸入值與輸出值的關係，判斷錯誤可以另外判斷。

### 為何要迴圈？
處理重複的事







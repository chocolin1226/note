###### tags: `Ruby & Rails`
# 20221110 Rails 筆記
[TOC]

---
<政庭>

##  Hash(雜湊)
* `old` h = {:name => "ccc", :age => 18} ---Ruby1.8
* `now` h = { name: "ccc", age: 18} ---Ruby1.9
```ruby
p h["name"]  /  p h[:name]
```
* 字串(String)跟符號(Symbol)的差別
* 符號就是"有名字的物件" 或是 符號就是一個"value"

`has_many(:books)`
`has_many(123)`
* 數字型態的 object_id 的規律 2N + 1
* 其他的型態 object_id 的規律 2N
```ruby
name = "cccc"
name[0] = "x"
p name
```
```ruby
name = :cccc
name[0] = "x"
p name

得到 undefined method '[]='。所以我們會說symbol不能更改它
```

## rails => ruby 加強版 好像魯夫 彈性很大
```ruby
params[:id]
params["id"] => nil
```
```rails
params[:id]
params["id"] 
兩種寫法都ok
```
## 課堂練習題
陣列 a = [1, 2, 3, 1, 2, 1, 3, 1, 2, 3, 4, 5, 6]，請計算在陣列 a 中，每個數字出現的次數。
```ruby
a = [1, 2, 3, 1, 2, 1, 3, 1, 2, 3, 4, 5, 6]

def count(arr)
    arr.map{ |a| arr.count(a) }
end
p count(a) // [4, 3, 3, 4, 3, 4, 3, 4, 3, 3, 1, 1, 1]
```
## 定義方法
method 跟物件有綁定
function 跟物件沒有綁定，它自己就是一個物件
在javascript可以說，呼叫某個物件的"方法"(function)。

## 參數 VS 引數
如果不知道兩者差別，那就通通記成"參數"
```ruby
def sayhello_to(someone)
    p "hello"
end

def hi
    p a = 1
end

sayhello_to
hi(someone)
```
## 〈冷知識〉
```ruby
age = 18

def age
    return 20
end

p age // 會得到什麼？
```
答案：
在Ruby的世界，會先找是否有定義 區域變數 或 方法
所以這題 p 印出來的結果會是 18


## 判斷方法的引數有幾個
link_to 

## 在Ruby裡面所有方法都有回傳值
```ruby
def hi
    1
    2
    3
end
這個方法回傳最後一個3並結束方法
```

## 問號 與 驚嘆號
```ruby
def is_adult?(age)
end
```
這裡暗示是要回傳 true / false
```ruby
def is_adult！(age)
end
```
這裡是要提醒這個方法有需要注意的地方
要去看手冊才知道驚嘆號是做了什麼

## 回傳 = 「交回控制權」

## 程式碼整理(模組化)
* 一個蘿蔔一個坑
* 盡量 一個方法寫在一個檔案裡面

## require VS load
`require "./檔案.rb"`   //有需要方法什麼就直接引入並執行檔案內容，但只會執行1次。

`load "./檔案.rb"`      //跟require的差別load幾次檔案，它就會執行幾次。

## 標準函式庫(Standard Libary)
Ruby 預設不會一起載入標準函式庫

## Rake
* Rake 是怎麼來的
* desc / namespace task 都有固定寫法
* 在linex Unix 系統 安裝套件時會輸入`make install`
* 所以在Ruby的世界，Rake = Ruby 的make ===> Rake
![](https://i.imgur.com/mqNAPpB.png)

### 命名空間(namespace)
* 可以把task做區分

### rake -T / rake --tasks
* 查詢在rakefile 裡面有哪些任務可以使用

## rails 5 之後將指令 rake db:migrate 改成 rails db:migrate

## Block(區塊)
block = 一段不會被“主動執行”的程式碼區塊
在Ruby世界裡面，block 不能單獨存在
block可以依附在其他方法後面，但是不會執行。這樣的目的是要做非同步執行。
除非前面的方法加上 `yield` 將控制權 "讓出" 交給後面的block。
```ruby
def hey
    yield 1,2
end

hey do |n,m|
    puts n
    puts m
end
```
這裡表示前面的 hey 方法會讓出 1,2 給後面的 do..end block，這個 block 會接那兩個引數並執行 block 裡面的程式碼，印出n ,印出m。
## block也會有回傳值
```ruby
def hi
    result = yield 2
    p result
end

hi do |x|
    x * 2
end
```
這樣的寫法，就是讓 hi 方法 可以接 block 回傳的值再印出來
等於是讓 block 來決定 hi 方法要印出的結果。
* 所以等於 block 讓 method 變得更有彈性。

## Open Class(開放類別)
我們可以擴充原本的 Class
會用到 self 

## 在 block 裡面不要加上return
* 等於在block裡面就直接結束了，不回會傳值給方法的yield。
```ruby
p list.filter(|x| x % 2 == 0)
p list.filter(|x| x.even?)
p list.filter(&:even?)
```
## bolck不是參數，要如何知道block有沒有存在？
```ruby
def hi
if block_given?
    yield
end
```
加上一行 `if block_given?` 有 block 依附在後面，才會 `yield`。 
## do-end 跟 { } 的差別
```ruby
p [1,2,3,4,5].map do |x|
    x * 2
end

p [1,2,3,4,5].map { |x|
    x * 2
}
```
do-end的結合率比較弱，所以第一個 p 要看成
```ruby
p([1,2,3,4,5].map) do |x| x * 2 end
```
第二個 p 看成
```ruby
p [1,2,3,4,5].map { |x| x * 2 }
```
這邊有發現到什麼嗎？
* 第一個 `p` 其實 `map` 被它包起來，所以後面的do-end等於完全沒有用了。 

## Proc (讓block可以被物件化，這樣block就能單獨存活，這樣就能被當作參數被丟來丟去)
```ruby
add_two = Proc.new { |x| x + 2 }

p add_two.call(3)  // 只要記得這個用法就好，必較常用。
#===================#
p add_two[3]
p add_two.(3)
p add_two.=== 3
p add_two === 3
p add_two.yield(3)
```
## Lambda (讓block可以被物件化，這樣block就能單獨存活，這樣就能被當作參數被丟來丟去)
```ruby
add_two = Lambda { |x| x + 2 }
add_two = -> (n) { |x| x + 2 }
```
# 在 Ruby 裡面 function 不是"物件"!!!

















---





<侑庭>


## hash
```ruby
舊是 h = {:name => 'aaa' , :age => 18}
糖衣 h = {name: 'aaa' , age: 18}

p h["name"]  #nil
p h[:name]   # aaa
```

##  symbol vs string
1. symbol 也是一種值,也是一個物件 :ccc <-冒號ccc 
2. `"string" .object_id *4  //40 60 80 100 `
3. `:symbol .object_id *4 // 188 188 188 188` 
4. 所有.object_id 的基數都會留給數字用,因為數字的id都會帶有2N+1的公式,所以都用單數
5. string 可視為字元陣列 可以直接用 name[0]去做取用但是這[]=也是一個方法

## 什麼時候要傳符號或是字串
看手冊,就會看到需要什麼,然後有些會有防呆機制但是就還是需要看手冊

## params 抓取物件

在controller 中很常看到
* `params[:id]`
* `params["id"]`

## method vs function

最簡單認定方式, method要有主詞 cat.eat()這樣

## 以下這段age會抓到什麼？

```ruby
age = 18 

def age
  retuen 20 
end

puts age  #印出18 
#如果沒有給他method or local virble他就什麼都不是 
#相對他會先找區域變數, 如果在age 後面加()就變成呼叫方法
```
## 回傳值
* 所有方法都有回傳值, 如果沒有給東西,回傳值就是nil
```ruby
def cat
end

p cat #nil 
```
* puts 寫在method 裡面是沒有回傳值的會直接印出來計算結果
* method 內都會自動回傳自己最後一行的結果

## 問號與驚嘆號
*  問號 跟 驚嘆號 都只能放在方法最後面
*  問號通則只會回傳true or false , 問號只是暗示不是rails 內建間的元素

*  驚嘆號就代表這方法可能會有不同的結果, 要看一下手冊
*  通常正常的跟不同的版本的都會綁在一起  


## 模組化
* 用 require './路徑位置' 就可以先把方法定義在不同檔案裡,在引入重複執行很多次的時候,require只會載入一次
 
* 用 load './路徑位置' 大致上相同跟require 只差在load 可以重複載入

## 標準函式庫 StdLib
* 不常用的方法所以不會在預設就載入而是會到,需要的時候再會去require近來


## 隨堂測驗







---

<侑庭> 下午場

## Rake
1. take 後面接hash的話會先做 value在做key ,task 設計問題
```ruby=
desc "text"
task :hi do 
    puts 'hello word'
end

#db  = datebass的意思 , namespace ＝ 命名空間,類似分類的感覺
namespace :db do    
  task :migrate do 
    puts "ok"
  end
end    

desc "hey!!"
task :hey => :hi do
    puts "hey"
end

task :default => :hi

# task (:default => :hi) #把小括號加回去
# task ({:default => :hi})#把大括號加回去
```

2. rake 目的是為了讓製作的時候可以新增新的方法

## Block
1. 他沒有辦法單獨存在,需要依附在一個物件後面才可以,but JS可以
2. 有兩種寫法 do..end 或 {}
3. 要能讀取到block 必須在方法裡面有要yield 在能暫時把控制權轉讓出去
4. 我們常在用loop method 例如 `list.map{|n| n2}` 這樣就是map裡面有yield 在map裡面
5. 在block裡面最後一行也會自動回傳
6. block的好處？ 我要任何結果都可以經過block去改變得到的答案,把彈性留在block裡面
7. block裡不能能有return 當有return過後就不會再繼續傳遞,在新版不會壞掉也不會錯,但在早期的版本就會跳出`LocaljumpError`
8. block 是一段不會被主動執行的程式碼
9. yield block_given? 可以用這方法去判斷有沒有使用block

## Block 物件化 ？
1. block 用 Proc.new 
2. block 用 lamnda -> {需要晚點執行的方法}
3. 呼叫Pron.new 要用add.call() , add[], add.() , add === 3
,add.yield(3) 這些方法呼叫
4. 因為在JS裡面的function可以隨時取用,但ruby內的block 不是物件所以無法提取, 所以才要用上面兩種方式去讓他物件化,這樣比較好去提取東西



## 隨堂解析
```ruby=
class Integer #更改
    alias :old_plus :+
    def +(n)
        self.old_plus(n)
    end
end
```

---


<于婷>

## 符號v.s.字串

符號：有名字的物件，是一種值

### 1. symbol的內容不能改變
  字串想要改其中一個字是可以的，但符號不行。
   ```
   "abcd"[0] = "z"    #字串變成"zbcd"
   :abcd[0] = "z"     #錯誤訊息
   ```
### 2. symbol指到同一個記憶體位置（編號）
  字串每次的記憶體位置是不固定的，而符號是同一個記憶體位置。
   ```
   5.times do
     puts "abcd".object_id
   end
   ```
   結果分別是：720 740 760 780 800
   ```
   5.times do
     puts :abcd.object_id
   end
   ```
   結果都是：1920348
   
   *每個人電腦的記憶體位置顯示都會有差異喔！
   我的例子是用Replit執行的結果。
   
   *補充：所有數字的位置都是奇數，其他放偶數。

### 3. symbol的效能比較好

   程式在比較符號是否相同時，
   是直接比對這物件的object_id是否相同，
   但在比較字串時，
   它是一個一個字母比對，
   因此比較的時間會隨著字母的數量而增加。
   
   *補充：但因為字串位置是暫存，長期來說，不會佔記憶體位置。
   
   
## hash v.s 陣列
1. 結構不同
2. 存取方式不同
3. 沒有順序

## 題目
```
a = [1, 2, 3, 1, 2, 1, 3, 1, 2, 3, 4, 5, 6]
#請計算在陣列 a 中，每個數字出現的次數。

p a.tally
```

## ruby的省略

為了更像人在講話

可「**適時**」省略（）{} return(最後一行的執行結果)

### 回傳值意義？

為了交回控制權

### ?與!
1. 放在物件後面
2. ?不成文默契：回傳true 或 false
3. !不成文默契：有你意想不到的結果，要注意


## Rakefile

```
desc "這是測試" #描述

task :hi do
    puts "hello world"
end

desc "hey123"
task :hey => :hi do #相依性
    puts "hey!!"
end

#初始
task :default => :hi 
```


## Block
不會主動執行的程式區塊，無法單獨存活。

## 題目

```
def my_map(arr)
  result = []
  arr.each { |x| result <<( yield x) }
  result
end

list = [1, 2, 3, 4, 5]
result = my_map(list) { |x| x * 2 }

p result # [2, 4, 6, 8, 10]
```

```
def my_filter(arr)
  result = []
  arr.each { |x| result << x if yield x }
  result
end

list = [1, 2, 3, 4, 5]
result = my_filter(list) { |x| x > 2 }

p result # [3, 4, 5]
```


## 有一些方法可以使block物件化：

### Proc

使用 Proc 類別把 Block 物件化
```
say_hello_to = Proc.new { |name| puts "hi #{name}"}  
```
呼叫Proc 方式
```
say_hello_to.call("小花")    # .call
say_hello_to.("小花")        # .小括號
say_hello_to["小花"]         # 中括號
say_hello_to === "小花"      # 三個等號
say_hello_to.yield "小花"    # .yield
```

### lambda

Proc 類別下，還有一個lambda可以把 Block 物件化，
但又有些許不同。

```
say_hello_to = lambda { |name| puts "hi #{name}"} 
```
或是
```
say_hello_to = ->(name) { puts "hi #{name}"} 
```
這兩種方式都可以把 Block 物件化，
呼叫方式和Proc 相同，
不過執行上會有些許不同


---




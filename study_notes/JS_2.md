

# [JavaScript]by value/by reference給值/給址

## by value:
**一般原始型別（primitive type：Boolean, String, Number, null, undefined）只是給值，不影響原本址的內容。**
let a = 1;
let b = a;
b = 2
console.log(a);
=>1
## by reference:
**物件型別（Objects type：Object, Array, Function）是給址，因此會影響原本址的內容。**
let a =[1];
let b = a;
b[0] = 2
console.log(a);
=>[2]

要注意的是
let a =[1];
let b = a;
b= [2]
console.log(a);
=>[1]

看得出差別嗎？
下面的例子b=[2]是重新給b一個新的址（新的陣列），
因此不會影響原本a的址的內容。
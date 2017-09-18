# Julia-Study

## [变量，类型和函数](http://www.justinablog.com/archives/1594)

### 变量 与 数值运算

```
julia> 3(1 + 2im)  # im  means imaginary part
3 + 6im
julia> 5//12 - 1//4      # // 表示分数
1//6
```
一些基本的复数操作函数：
实数运算
```
julia> real(1 + 2im)
1
```
共轭复数  Conjugate Complex
```
julia> conj(1 + 2im)
1 - 2im
```

ans这个变量指向上下文最后一个值，无论有没有变量指向它:
```
julia> ans  
3 + 6im
julia> 4
4
julia> ans
4
```

### 函数
标准定义以end结尾，赋值形式更简洁,julia支持**复合表达式**
```
# 标准定义
function f(x,y)
  x + y
end
# 赋值形式
f(x,y) = x + y
#复合表达式
julia> f(x,y) = (if x < y x + y else x -y end) 
f (generic function with 1 method) 
julia> f(1,3)
4
julia> f(4,3)
1
```
函数调用提供 **apply()** 方法 *【虽然我不知道这个方法有啥好】*
```
# apply
julia> apply(f,5,3)
2
# 运算符即函数
julia> +(5,3)
8
#支持多值返回
julia> foo(a,b) = a+b, a*b
julia> foo(5,3)
(8,15)
```

**可变参数函数**可传入任意个数参数，名称由...定义：
```
julia> bar(a,b,x...) = (a,b,x)
julia> bar(1,2)
(1,2,())
julia> bar(1,2,3)
(1,2,(3,))
julia> bar(1,2,3,4,5,6)
(1,2,(3,4,5,6))
```

**可选参数**的定义和Python语法非常接近：*【我还不知道这是什么鬼】*
*【我现在知道了，大概是调用函数时可以忽略这个可选参数，不一定要用到？】*
```
function parseint(num, base=10)
   ###
end
```

## [多维数组](http://www.justinablog.com/archives/1604)

### 数组

对Julia语言本身来说，它只提供了数组，更高抽象度的实现如TimeSeries和DataFrames，在另一个专门面向统计和机器学习的项目Julia Statistics中，需要通过添加Package的方式进行引用。


基本数组， Julia会根据输入数据判断数组类型，如果数值与**字符串**都存在，则是any类型; But, 如果是单个字符的话，则会转化成另一个成员的类型；可以使用大括号主动创建一个 any 类型的数组
```
julia> l = [3, 4, 5]
3-element Array{Int64,1}:
3
4
5
julia> l = [3., "a"]
2-element Array{Any,1}:
3.0
"a"
julia> l = [3., 'a']
2-element Array{Float64,1}:
3.0
97.0
julia> l = { 3., "a", [3, 4] }
3-element Array{Any,1}:
3.0
"a"
[3,4]
```
### 索引
Julia的索引是从1开始的; 使用end来标识索引的尾部 
```
julia> l[1]
3.0
julia> l[1:2]
2-element Array{Any,1}:
3.0
"a"
julia> l[2:end]
2-element Array{Any,1}:
"a"
[3,4]
```
### 数组操作
- Julia使用 push! 函数将成员追加到数组末尾，不支持追加不同类型的成员，但小数点为0的浮点类型可以自动转换。
- append! 函数则将一个数组的所有成员追加到原始数组中
- 对于数组，可直接使用加减乘除运算符进行操作。
- 对于数组和矩阵的乘法，Julia采用了Matlab的语法，使用 .* 
```
julia> l = [3, 4, 5]
julia> push!(l, 12.0)
4-element Array{Int64,1}:
3
4
5
12
julia> append!(l, [10, 11, 12])
7-element Array{Int64,1}:
3
4
5
12
10
11
12
julia> a = [1.1, 2.2, 3.3]
julia> b = [4.4, 5.5, 6.6]
julia> a + b
5.5
7.7
9.9
julia> a .* b
4.84
12.1
21.78
```

### 向量和矩阵
- 向量可以视为 m x n 矩阵 m = 1 的情况，Jilia中的数组的表现特点可与向量等同。定义一个向量是使用不带逗号的“数组”
- 矩阵的转置是使用 ‘ 来完成，对于复数矩阵，可以使用 .’ 直接完成共轭复数转置。*[这条是骗子吧，共轭转置我没试成功啊？】*
```
julia> vec = [3 4 5]
1x3 Array{Int64,2}:
3 4 5
julia> M = [1 2; 3 4; 5 6]
3x2 Array{Int64,2}:
1 2
3 4
5 6
julia> M'
2x3 Array{Int64,2}:
1 3 5
2 4 6
```
数组可转化成向量：
```
julia> l = [3, 4, 5]
3-element Array{Int64,1}:
3
4
5
julia> l' # 数组通过转置变成了向量
1x3 Array{Int64,2}:
3 4 5
julia> l'' # 再次转置变成了一个矩阵
3x1 Array{Int64,2}
3
4
5
```
***
***
***
## [Follow the Manual](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/variables/)
### 一、变量
1. 变量的意义：存储一个值以后备用
2. Julia中变量名区分大小写，也可以使用Unicode（utf-8)来命名；其中unicode中一些数学符号只需要输入对应的Latex语句，然后按Tab完成输入，e.g.变量名 δ 可以通过 \delta-tab 来输入，又如 α̂₂可以由 \alpha-tab-\hat-tab-\_2-tab 来完成。
3. 允许重新定义内置的常数和函数，如pi和sqrt()，但不鼓励
4. 遵守一定的命名规范：
   - 变量名使用小写字母
   - 单词间使用下划线分隔，但不鼓励
   - 类型名首字母大写, 单词间使用驼峰式分隔.
   - 函数名和宏名使用小写字母, 不使用下划线分隔单词.
   - 修改参数的函数结尾使用 ! . 这样的函数被称为 mutating functions 或 in-place functions *【不懂~】*

### 二、整数和浮点数
#### 整数
1. 整数文本的默认类型，取决于目标系统是 32 位架构还是 64 位架构，对于不能用 32 位而只能用 64 位来表示的大整数文本，不管系统类型是什么，始终被认为是 64 位整数
2. 基础数值类型的最小值和最大值，可由 typemin 和 typemax 函数查询（浮点数也适用这两个函数）：
```
julia> (typemin(Int32), typemax(Int32))
(-2147483648,2147483647)

julia> for T = {Int8,Int16,Int32,Int64,Int128,Uint8,Uint16,Uint32,Uint64,Uint128}
         println("$(lpad(T,7)): [$(typemin(T)),$(typemax(T))]")
       end
   Int8: [-128,127]
  Int16: [-32768,32767]
  Int32: [-2147483648,2147483647]
  Int64: [-9223372036854775808,9223372036854775807]
 Int128: [-170141183460469231731687303715884105728,170141183460469231731687303715884105727]
  Uint8: [0,255]
 Uint16: [0,65535]
 Uint32: [0,4294967295]
 Uint64: [0,18446744073709551615]
Uint128: [0,340282366920938463463374607431768211455]
```
3. 注意**溢出**和**除法错误**
4. 数字分隔符可以使数字位数更加清晰,建议大家在写多位数字的时候，使用数字分隔符。
```
julia> a = 100_000
100000

julia> a = 100_0000_0000
10000000000
```

#### 浮点数
1. 浮点数类型中存在 两个零 ，正数的 零和负数的零。它们相等，但有着不同的二进制表示，可以使用 bits 函数看出：
```
julia> 0.0 == -0.0
true

julia> bits(0.0)
"0000000000000000000000000000000000000000000000000000000000000000"

julia> bits(-0.0)
"1000000000000000000000000000000000000000000000000000000000000000"
```
2. 有三个特殊的标准浮点数：Inf 正无穷 -Inf 负无穷 NaN 不存在（不等于、不大于、不小于任何数，包括它本身）
#### 精度
大多数的实数并不能用浮点数精确表示，因此有必要知道两个相邻浮点数间的间距，也即 计算机的精度*【去查查中文的解释!!】* 
Julia 提供了 eps 函数，可以用来检查 1.0 和下一个可表示的浮点数之间的间距：
```
julia> eps(Float32)
1.1920929f-7

julia> eps(Float64)
2.220446049250313e-16

julia> eps() # same as eps(Float64)
2.220446049250313e-16
```
eps 函数也可以取浮点数作为参数，给出这个值和下一个可表示的浮点数的绝对差，即， eps(x) 的结果与 x 同类型，且满足 x + eps(x) 是下一个比 x 稍大的、可表示的浮点数：*【类似于微小值】*
```
julia> eps(1.0)
2.220446049250313e-16

julia> eps(1000.)
1.1368683772161603e-13

julia> eps(1e-27)
1.793662034335766e-43

julia> eps(0.0)
5.0e-324
```
相邻的两个浮点数之间的距离并不是固定的，数值越小，间距越小；数值越大, 间距越大。换句话说，浮点数在 0 附近最稠密，随着数值越来越大，数值越来越稀疏，数值间的距离呈指数增长。根据定义， eps(1.0) 与 eps(Float64) 相同，因为 1.0 是 64 位浮点数。

函数 nextfloat 和 prevfloat 可以用来获取下一个或上一个浮点数：
```
julia> x = 1.25f0
1.25f0

julia> nextfloat(x)
1.2500001f0

julia> prevfloat(x)
1.2499999f0
```
##### 与精度计算有关的问题
1. 舍入模型
如果一个数没有精确的浮点数表示，那就需要舍入了。可以根据 IEEE 754 标准 来更改舍入的模型；默认舍入模型为 RoundNearest ，它舍入到最近的可表示的值，这个被舍入的值使用尽量少的有效数字。
```
julia> 1.1 + 0.1
1.2000000000000002

julia> with_rounding(Float64,RoundDown) do
       1.1 + 0.1
       end
1.2
```
2. 任意精度的算术
Julia 相应提供了 BigInt 和 BigFloat 类型。可以通过基础数值类型或 String 类型来构造
```
julia> BigInt(typemax(Int64)) + 1
9223372036854775808

julia> BigInt("123456789012345678901234567890") + 1
123456789012345678901234567891

julia> BigFloat("1.23456789012345678901")
1.234567890123456789010000000000000000000000000000000000000000000000000000000004e+00 with 256 bits of precision
```
但是基础数据类型和 BigInt/BigFloat **不能自动**进行类型转换，需要明确指定。
```
julia> x = typemin(Int64)
-9223372036854775808

julia> x = x - 1
9223372036854775807

julia> typeof(x)
Int64

julia> y = BigInt(typemin(Int64))
-9223372036854775808

julia> y = y - 1
-9223372036854775809

julia> typeof(y)
BigInt (constructor with 10 methods)
```
BigFloat 运算的默认精度（有效数字的位数）和舍入模型，是可以改的。然后，计算就都按照更改之后的设置来运行了：

#### 代数系统

Julia 允许在变量前紧跟着数值文本，来表示乘法。这有助于写多项式表达式。数值文本系数同单目运算符一样。因此 2^3x 被解析为 2^(3x) ， 2x^3 被解析为 2\*(x^3)。**需要注意**，代数因子和变量或括号表达式之间不能有空格。
```
julia> x = 3
3

julia> 2x^2 - 3x + 1
10

julia> 2^2x
64
```
关于括号：
- 数值文本可以作为括号表达式的因子,括号表达式可作为变量的因子,但是不要接着写两个变量括号表达式，也不要把变量放在括号表达式之前。它们不能被用来指代乘法运算 【括号放后面表示函数调用，所以会出错】
```
julia> 2(x-1)^2 - 3(x-1) + 1
3
julia> (x-1)x
6
julia> (x-1)(x+1)
ERROR: type: apply: expected Function, got Int64
julia> x(x+1)
ERROR: type: apply: expected Function, got Int64
```
#### 零和一
Julia 提供了一些函数, 用以得到特定数据类型的零和一文本。这俩函数在数值比较中可用来避免额外的类型转换。*【嗯，还没有get到这个的用处】*
```
julia> zero(Float32)
0.0f0

julia> zero(1.0)
0.0

julia> one(Int32)
1

julia> one(BigFloat)
1.000000000000000000000000000000000000000000000000000000000000000000000000000000
```
### 三、数学运算和基本函数
#### 算数运算符
- x/y 除法  x\y反除  
- 习惯上，优先级低的运算，前后多补些空格。这不是强制的。
- 支持复合赋值运算符（+=）
#### [位运算符](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/mathematical-operations/#id4)
略
#### 数值比较
NaN 在 矩阵 中使用时会带来些麻烦：
```
julia> [1 NaN] == [1 NaN]
false
```
因此Julia 提供了附加函数, 用以测试这些特殊值，它们使用哈希值来比较:
isequal(x, y)	 x 是否等价于 y
isfinite(x)	   x 是否为有限的数
isinf(x)	     x 是否为无限的数
isnan(x)	     x 是否不是数
```
julia> isequal(NaN,NaN)
true

julia> isequal([1 NaN], [1 NaN])
true

julia> isequal(NaN,NaN32)
true

julia> -0.0 == 0.0
true

julia> isequal(-0.0, 0.0)
false
```
- isequal 函数，认为 NaN 等于它本身
- isequal 也可以用来区分有符号的零
#### 链式比较
```
julia> 1 < 2 <= 2 < 3 == 3 > 2 >= 1 == 1 < 3 != 5
true
```
- 操作符 .< 是特别针对数组的; 只有当 A 和 B 有着相同的大小时, A .< B 才是合法的. 比较的结果是布尔型数组, 其大小同 A 和 B 相同.  0 .< A .< 1 的结果是一个对应的布尔数组，满足条件的元素返回 true 
- 注意链式比较的比较顺序, 链式比较的计算顺序是不确定的。不要在链式比较中使用带副作用（比如打印）的表达式。如果需要使用副作用表达式，推荐使用短路 && 运算符
```
v(x) = (println(x); x)

julia> v(1) < v(2) <= v(3)
2
1
3
true

julia> v(1) > v(2) <= v(3)
2
1
false
```
#### [基本函数](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/mathematical-operations/#man-elementary-functions)
略

### 四、复数和分数
#### 复数
- 代数系数不能用于使用变量构造复数。乘法必须显式的写出来, 但是， **不推荐**使用这个方法。推荐使用 complex 函数构造复数:
```
julia> a = 1; b = 2; a + b*im
1 + 2im

julia> complex(a,b)
1 + 2im
```
#### 分数
- 使用 // 运算符构造分数
- 如果分子、分母有公约数，将自动约简至最简分数，且分母为非负数
- 使用 num 和 den 函数来取得约简后的分子和分母
- 分数可以简单地转换为浮点数float()
```
julia> 6//9
2//3
julia> 5//-15
-1//3
```

### 五、字符串
#### 字符
- Char 表示单个字符：它是 32 位整数，值参见 Unicode 码位;Char 必须使用单引号; 可以把 Char 转换为对应整数值
- 可以把 Char 转换为对应整数值,并非所有的整数值都是有效的 Unicode 码位，但为了性能， Char 一般不检查其是否有效。如果你想要确保其有效，使用 isvalid 函数
```
julia> 'x'
'x'
julia> typeof(ans)
Char

julia> Int('x')
120
julia> typeof(ans)
Int64

julia> Char(0x110000)
'\U110000'
julia> isvalid(Char, 0x110000)
false
```
#### 字符串基础
- 字符串文本放在**一对或三对儿**双引号之间
- 使用索引从字符串提取字符; 在任何索引表达式中，关键词 end 都是最后一个索引值（由 endof(str) 计算得到）的缩写。可以对字符串做 end 算术或其它运算
```
julia> str = "Hello, world.\n"
"Hello, world.\n"
julia> str[end]
'\n'
julia> str[end-1]
'.'
```
- 使用范围索引来提取子字符串. str\[k] 和 str\[k:k] 的结果不同, 前者是类型为 Char 的单个字符，后者为仅有一个字符的字符串。在 Julia 中这两者完全不同。
```
julia> str[4:9]
"lo, wo"
julia> str[6]
','
julia> str[6:6]
","
```
#### [Unicode 和 UTF-8](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/strings/#unicode-utf-8)
略
#### [内插](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/strings/#man-string-interpolation)
- 字符串的连接和内插：
```
julia> greet = "Hello"
"Hello"
julia> whom = "world"
"world"
julia> string(greet, ", ", whom, ".\n")
"Hello, world.\n"

julia> "$greet, $whom.\n"
"Hello, world.\n"
julia> "1 + 2 = $(1 + 2)"
"1 + 2 = 3"
```
- 字符串连接和内插都调用 string 函数来把对象转换为 String 。与在交互式会话中一样，大多数非 String 对象被转换为字符串;要在字符串文本中包含 $ 文本，应使用反斜杠将其转义
```
julia> v = [1,2,3]
3-element Array{Int64,1}:
 1
 2
 3
julia> "v: $v"
"v: [1,2,3]"

julia> print("I have \$100 in my account.\n")
I have $100 in my account.
```
#### 一般操作
1. 比较：使用标准比较运算符，按照字典顺序比较字符串
2. 查找：使用search()函数查找某个字符的索引值；可以通过提供第三个参数，从此偏移值开始查找
3. 重复：repeat()
```
julia> "abracadabra" < "xylophone"
true
julia> "Hello, world." != "Goodbye, world."
true

julia> search("xylophone", 'p')
5
julia> search("xylophone", 'o')
4
julia> search("xylophone", 'o', 5)
7
julia> search("xylophone", 'o', 8)
0

julia> repeat(".:Z:.", 10)
".:Z:..:Z:..:Z:..:Z:..:Z:..:Z:..:Z:..:Z:..:Z:..:Z:."
```
#### [非标准字符串文本](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/strings/#man-non-standard-string-literals)
略

### 六、[函数](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/functions/)
Julia 中的函数是将一系列**参数组成的元组**映设到一个返回值的对象，Julia 的函数**不是纯的数学式函数**，有些函数可以改变或者影响程序的全局状态。
- 定义函存在基本语法和赋值形式两种；对于赋值形式，函数体通常是单表达式，但也可以为复合表达式。
```
function f(x,y)
  x + y
end

f(x,y) = x + y
```
- 使用圆括号来调用函数; 没有圆括号时， f 表达式指向的是函数对象，这个函数对象可以像值一样被传递：
```
julia> f(2,3)
5

julia> g = f;
julia> g(2,3)
5
```
- 调用函数有两种方法：使用特定函数名的特殊运算符语法，或者使用 apply 函数(apply 函数把第一个参数当做函数对象，应用在后面的参数上)
```
julia> apply(f,2,3)
5
```
- Julia 函数的参数遵循 “pass-by-sharing” 的惯例，即不传递值，而是传递引用。
#### return关键字
函数返回值通常是函数体中最后一个表达式的值。对比下列两个函数：
```
f(x,y) = x + y
function g(x,y)
  return x * y
  x + y
end

julia> f(2,3)
5
julia> g(2,3)
6
```
在纯线性函数体*【我理解就是只执行end前的那个式子？】*，比如 g 中，不需要使用 return ，可以把 x * y 作为函数的最后一个表达式，并省略 return 。只有涉及其它控制流时， return 才有用。比如：
```
function hypot(x,y)
  x = abs(x)
  y = abs(y)
  if x > y
    r = y/x
    return x*sqrt(1+r*r)
  end
  if y == 0
    return zero(x)
  end
  r = x/y
  return y*sqrt(1+r*r)
end
```
#### 函数运算符
Julia 中，大多数运算符都是支持特定语法的函数。 && 、 || 等短路运算是例外，它们不是函数，因为**短路求值** 先算前面的值，再算后面的值。 对于函数运算符，可以像其它函数一样，把参数列表用圆括号括起来，作为函数运算符的参数：
```
julia> +(1,2,3)
6
julia> f = +;
julia> f(1,2,3)
6
```
#### 匿名函数 [http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/functions/#man-anonymous-functions]
#### 多返回值
#### 变参函数
#### 可选参数
#### 关键字参数
#### 默认值的求值作用域
#### 函数参数的块语法

### 七、[控制流](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/control-flow/)
Julia 提供一系列控制流：
- 复合表达式 ： begin 和 (;)
- 条件求值 ： if-elseif-else 和 ?: (ternary operator)
- 短路求值 ： &&, || 和 chained comparisons
- 重复求值: 循环 ： while 和 for
- 异常处理 ： try-catch ， error 和 throw
- 任务（也称为协程） ： yieldto
前五个控制流机制是高级编程语言的标准。但任务不是：它提供了非本地的控制流，便于在临时暂停的计算中进行切换。在 Julia 中，异常处理和协同多任务都是使用的这个机制。
#### 复合表达式
用一个表达式按照顺序对一系列子表达式**求值**，并**返回最后一个**子表达式的值，有两种方法： begin 块和 (;) 链。 
begin 块的例子：
```
julia> z = begin
         x = 1
         y = 2
         x + y
       end
3
```
 可以用(;) 链语法将其放在一行上(这个语法在 函数 中的单行函数定义非常有用)：
 ```
julia> z = (x = 1; y = 2; x + y)
3
 ```
begin 块也可以写成单行， (;) 链也可以写成多行：*【这个也太强行了吧……一点都没有美感】*
```
julia> begin x = 1; y = 2; x + y end
3
julia> (x = 1;
        y = 2;
        x + y)
3
```
#### 条件求值
if-elseif-else*【跟各家语言都一样】*
**但是**，如果条件表达式的值是除 true 和 false 之外的值，会出错：
```
julia> if 1
         println("true")
       end
ERROR: type: non-boolean (Int64) used in boolean context
```
“问号表达式”语法 ?:    a ? b : c
三选一的例子需要链式调用问号表达式;链式问号表达式的结合规则是从右到左*【就是从左到右写，更好理解一些】*
```
julia> test(x, y) = println(x < y ? "x is less than y"    :
                            x > y ? "x is greater than y" : "x is equal to y")
```
#### 短路求值
&& 和 || 布尔运算符被称为短路求值，它们连接一系列布尔表达式，仅计算最少的表达式来确定整个链的布尔值。这意味着：
- 在表达式 a && b 中，只有 a 为 true 时才计算子表达式 b
- 在表达式 a || b 中，只有 a 为 false 时才计算子表达式 b
&& 和 || 都与右侧结合，但 && 比 || 优先级高：



### 八、[变量的作用域](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/variables-and-scoping/)
### 九、[类型](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/types/)
### 十、[方法](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/methods/)
### 十一、[构造函数](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/constructors/)
### 十二、[类型转换和类型提升](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/conversion-and-promotion/)
### 十三、[模块](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/modules/)
### 十四、[元编程](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/metaprogramming/)
### 十五、[多维数组](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/arrays/)
### 十六、[线性代数](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/linear-algebra/)
### 十七、[多维数组](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/arrays/)
### 十八、[线性代数](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/linear-algebra/)
### 十九、[代码性能优化](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/performance-tips/)
### 二十、[代码样式](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/style-guide/)
### 二十一、[与其他语言的区别](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/noteworthy-differences/)


### 跳过：高级部分（网络和流，并行计算，包，宏等等……）

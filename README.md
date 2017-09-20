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
#### 内插
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

### 六、函数
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
#### 匿名函数
匿名函数的主要作用是把它传递给接受其它函数作为参数的函数。
```
julia> x -> x^2 + 2x - 1
(anonymous function)
```
最经典的例子是 map 函数，它将函数应用在数组的每个值上，返回结果数组。map 的第一个参数可以是非匿名函数。但是大多数情况，不存在这样的函数时，匿名函数就可以简单地构造单用途的函数对象，而不需要名字：
```
julia> map(x -> x^2 + 2x - 1, [1,3,-1])
3-element Array{Int64,1}:
  2
 14
 -2
```
匿名函数可以通过类似 (x,y,z)->2x+y-z 的语法接收多个参数。**无参匿名函数**则类似于 ()->3 。
无参匿名函数可以“延迟”计算，做这个用处时，代码被封装进无参函数，以后可以通过把它命名为 f() 来引入。*【这个延迟还不太懂哦！！！】*
#### 多返回值
通过返回多元组来模拟返回多值。但是，多元组并不需要圆括号来构造和析构，因此造成了可以返回多值的假象。
```
julia> function foo(a,b)
         a+b, a*b    
         #也可以通过 return a+b, a*b 达到相同的目的
       end;
julia> foo(2,3)
(5,6)
```
Julia 支持简单的多元组“析构”来给变量赋值：
```
julia> x, y = foo(2,3);

julia> x
5

julia> y
6
```
#### 变参函数
函数的参数列表如果可以为任意个数，有时会非常方便。这种函数被称为“变参”函数，是“参数个数可变”的简称。可以在最后一个参数后紧跟省略号 ... 来定义变参函数。变量 a 和 b 是前两个普通的参数，变量 x 是尾随的可迭代的参数集合，其参数个数为 0 或多个:
```
julia> bar(a,b,x...) = (a,b,x)
bar (generic function with 1 method)

julia> bar(1,2)
(1,2,())
julia> bar(1,2,3)
(1,2,(3,))
julia> bar(1,2,3,4)
(1,2,(3,4))
```
上述例子中， x 是传递给 bar 的尾随的值多元组。
函数调用时，也可以使用 ... ;多元组的值也可以不完全遵守其函数定义来调用;被内插的对象也可以不是多元组；
```
julia> x = (3,4)
(3,4)
julia> bar(1,2,x...)   #按照定义
(1,2,(3,4))

julia> x = (2,3,4)   #不按定义
(2,3,4)
julia> bar(1,x...)
(1,2,(3,4))

julia> x = [3,4]      #不是多元组
2-element Array{Int64,1}:
 3
 4
julia> bar(1,2,x...)
(1,2,(3,4))
```
原函数也可以不是变参函数（大多数情况下，应该写成变参函数），但如果输入的参数个数不对，函数调用会失败。
```
baz(a,b) = a + b

julia> args = [1,2]
2-element Int64 Array:
 1
 2
julia> baz(args...)
3

julia> args = [1,2,3]
3-element Int64 Array:
 1
 2
 3
julia> baz(args...)
no method baz(Int64,Int64,Int64)
```
#### 可选参数
很多时候，函数参数都有默认值。例如，库函数 parseint(num,base) 把字符串解析为某个进制的数。 base 参数默认为 10 。这时，调用函数时，参数可以是一个或两个。当第二个参数未指明时，自动传递 10 。
```
function parseint(num, base=10)
    ###
end

julia> parseint("12",10)
12
julia> parseint("12")
12
```
#### 关键字参数
有些函数的参数有很多，关键字参数允许使用参数名来区分和使用这些参数。
使用关键字参数的函数，在函数签名中使用分号来定义;额外的关键字参数，可以像变参函数中一样，使用 ... 来匹配
```
function plot(x, y; style="solid", width=1, color="black")
    ###
end
function f(x; y=0, args...)
    ###
end
```
关键字参数的默认值仅在必要的时候从左至右地被求值(当对应的关键字参数没有被传递)，所以默认的(关键字参数的)表达式可以调用在它之前的关键字参数。

#### 默认值的求值作用域
可选参数和关键字参数的区别在于它们的默认值是怎样被求值的。当可选的参数被求值时，只有在它**之前的**的参数在作用域之内; 与之相对的, 当关键字参数的默认值被计算时, **所有的参数**都是在作用域之内。比如，定义函数:
```
function f(x, a=b, b=1)
    ###
end
```
在 a=b 中的 b 指的是该函数的作用域之外的 b ，而不是接下来 的参数 b。然而，如果 a 和 b 都是关键字参数，那么它们都将在 生成在同一个作用域上， a=b 中的 b 指向的是接下来的参数 b (遮蔽了任何外层空间的 b), 并且 a=b 会得到未定义变量的错误 (因为**默认参数的表达式是自左而右的求值的**，b并没有被赋值)。
#### 函数参数的块语法
将函数作为参数传递给其它函数，当行数较多时，有时不太方便。Julia 提供了保留字 do 来重写这种代码，使之更清晰：
```
map(x->begin
           if x < 0 && iseven(x)
               return 0
           elseif x == 0
               return 1
           else
               return x
           end
       end,
    [A, B, C])
################################
map([A, B, C]) do x
    if x < 0 && iseven(x)
        return 0
    elseif x == 0
        return 1
    else
        return x
    end
end
```
do x 语法会建立一个以 x 为参数的匿名函数，并将其作为第一个参数传递给 map . 类似地， do a,b 会创造一个含双参数的匿名函数，而一个普通的do将声明其后是一个形式为() -> ...的匿名函数。
#### [延伸阅读](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/functions/#id14)
关于多重派发（略）

### 七、控制流
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
```
julia> t(x) = (println(x); true)
t (generic function with 1 method)
julia> f(x) = (println(x); false)
f (generic function with 1 method)
julia> f(1) && t(2)
1
false
julia> t(1) || f(2)
1
true
```
这种方式在 Julia 里经常作为短 if 语句的一个简洁的替代。可以把 if <cond> <statement> end 写成 <cond> && <statement> (读作 <cond> 从而 <statement>)。 类似地，可以把 if ! <cond> <statement> end 写成 <cond> || <statement> (读作 <cond> 要不就 <statement>)。
  
*【感觉这个会很方便哦~】*
例如, 递归阶乘可以这样写:
```
julia> function factorial(n::Int)
           n >= 0 || error("n must be non-negative")   #注意error的写法哦~
           n == 0 && return 1
           n * factorial(n-1)
       end
factorial (generic function with 1 method)
```
- 短路求值的运算对象也必须是布尔值（1,0都不行）；除了最后一项外，在短路求值中使用非布尔值是一个错误：短路求值的最后一项可以是任何类型的表达式。取决于之前的条件，它可以被求值并返回。
- **非短路**求值运算符，可以使用位布尔运算符 & 和 | ：
```
julia> f(1) & t(2)
1
2
false

julia> t(1) | t(2)
1
2
true
```
#### 重复求值：循环
- while循环和for循环，可互相改写*【也是大同小异咯】*
- while 循环和 for 循环的一区别是变量的作用域。如果在其它作用域中没有引入变量 i ，那么它仅存在于 for 循环中。
```
julia> i = 1;
julia> while i <= 5
         println(i)
         i += 1
       end
julia> for i = 1:5
         println(i)
       end
```
for循环中 1:5 是一个range对象，表示一个序列；for循环遍历这个序列，把值逐一赋给i;通常，for循环可以遍历任意容器。这时，应使用另一个（但是完全等价的）关键词 in ，而不是 = ，它使得代码更易阅读：
```
julia> for i in [1,4,0]
         println(i)
       end
1
4
0
```
多层 for 循环可以被**重写为一个外层循环**:
```

julia> for i = 1:2, j = 3:4
         println((i, j))
       end
```
**关键字**break和continue，分别用于**提前终止循环**和**中断本次循环，进行下一次循环**
但如果在多层for循环被写成外层循环的情况下使用break会直接跳出所有循环，而不仅仅是最内层循环。
```
julia> i = 1;
julia> while true
         println(i)
         if i >= 5
           break
         end
         i += 1
       end
1
2
3
4
5

julia> for i = 1:10
         if i % 3 != 0
           continue
         end
         println(i)
       end
3
6
9
```
#### 异常处理
**内置异常 exception**：如果程序遇到意外条件，异常将会被抛出。表中列出内置异常（略）。可以自己定义自己的异常：
```
julia> type MyCustomException <: Exception end   #【那个符号是什么鬼？？谁来告诉我~我猜是导出的意思】
```
**throw 函数**
可以使用 throw 函数显式创建异常。例如，某个函数只对非负数做了定义，如果参数为负数，可以抛出 DomaineError 异常
```
julia> f(x) = x>=0 ? exp(-x) : throw(DomainError())
f (generic function with 1 method)
```
- 注意， DomainError 使用时需要使用带括号的形式，否则返回的并不是异常，而是异常的类型。必须带括号才能返回 Exception 对象
```
julia> typeof(DomainError()) <: Exception
true

julia> typeof(DomainError) <: Exception
false
```
- 某些异常接受参数，在自定义异常中，也可以设置参数：
```
julia> throw(UndefVarError(:x))
ERROR: x not defined

julia> type MyUndefVarError <: Exception   #自定义
           var::Symbol
       end
julia> Base.showerror(io::IO, e::MyUndefVarError) = print(io, e.var, " not defined");
```
**error函数**
error 函数用来产生 ErrorException ，阻断程序的正常执行。如下改写 sqrt 函数,当对负数调用fussy_sqrt 时，它会立即返回，显示错误信息
```
julia> fussy_sqrt(x) = x >= 0 ? sqrt(x) : error("negative x not allowed")
fussy_sqrt (generic function with 1 method)

julia> function verbose_fussy_sqrt(x)
         println("before fussy_sqrt")
         r = fussy_sqrt(x)
         println("after fussy_sqrt")
         return r
       end
verbose_fussy_sqrt (generic function with 1 method)

julia> verbose_fussy_sqrt(2)
before fussy_sqrt
after fussy_sqrt
1.4142135623730951

julia> verbose_fussy_sqrt(-1)
before fussy_sqrt
ERROR: negative x not allowed
 in verbose_fussy_sqrt at none:3
```
**warn和info函数**
用来向标准错误 I/O 输出一些消息，但不抛出异常，因而并不会打断程序的执行：
```
julia> info("Hi"); 1+1
INFO: Hi
2

julia> warn("Hi"); 1+1
WARNING: Hi
2

julia> error("Hi"); 1+1
ERROR: Hi
 in error at error.jl:21
```
**try/catch语句**
try/catch 语句可以用于处理一部分预料中的异常 Exception 。例如，下面求平方根函数可以正确处理实数或者复数([但是处理异常比正常采用分支来处理，会慢得多。])：
```
julia> f(x) = try
         sqrt(x)
       catch
         sqrt(complex(x, 0))
       end
f (generic function with 1 method)

julia> f(1)
1.0
julia> f(-1)
0.0 + 1.0im
```
try/catch 语句使用时也可以把异常赋值给某个变量。例如：
```
julia> sqrt_second(x) = try
         sqrt(x[2])
       catch y
         if isa(y, DomainError)    #isa(): 判断输入参量是否为指定类型的对象
           sqrt(complex(x[2], 0))
         elseif isa(y, BoundsError)
           sqrt(x)
         end
       end
sqrt_second (generic function with 1 method)
```
注意，紧跟 catch 的符号会作为异常的名字，所以在将 try/catch 写在单行内的时候需要特别注意。下面第一行代码不会在发生错误的时候返回 x 的值;相对的，使用分号或在 catch 后另起一行:
```
try bad() catch x end

try bad() catch; x end   #【那这种还catch到东西了咩？？？？？】
try bad()
catch
  x
end
```
**finally语句**
在改变状态或者使用文件等资源时，通常需要在操作执行完成时做清理工作（比如关闭文件）。异常的存在使得这样的任务变得复杂，因为异常会导致程序提前退出。关键字 finally 可以解决这样的问题，**无论程序是怎样退出的， finally 语句总是会被执行。**
```
f = open("file")
try
    # operate on file f
finally
    close(f)
end
```
#### [任务（也称为协程)](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/control-flow/#man-tasks)
略（宝宝实在看不懂）

### 八、变量的作用域
作用域是变量可见的区域,能帮助避免变量命名冲突。
**作用域块** 是作为变量作用域的代码区域。变量的作用域被限制在这些块内部。作用域块有：
- function 函数体（或 语法 ）- while 循环体 - for 循环体 - try 块 - catch 块 - let 块 - type 块
注意: **begin块不能**引入新作用域块。

当变量被引入到一个作用域中时，所有的**内部作用域都继承**了这个变量，除非某个内部作用域显式复写了它。将新变量引入当前作用域的方法：
- 声明 local x 或 const x ，可以引入新本地变量
- 声明 global x 使得 x 引入当前作用域和更内层的作用域
- 函数的参数，作为新变量被引入函数体的作用域
- 无论是在当前代码之前或 之后 ， x = y 赋值都将引入新变量 x ，除非 x 已经在任何外层作用域内被声明为全局变量或被引入为本地变量
- 在非顶层作用域给全局变量赋值的唯一方法，是在某个作用域中显式声明变量是全局的。否则，赋值会引入新的局部变量，而不是给全局变量赋值。

下例中，循环体有一个独立的 x ，函数始终返回 0 ：
```
function foo(n)
  x = 0
  for i = 1:n
    local x
    x = i
  end
  x
end

julia> foo(10)
0
```
不必在内部使用前，就在外部赋值引入 x ：
```
function foo(n)
  f = y -> n + x + y   # ->表示引用 f为一个匿名函数 y为input n+x+y为output  （by 小亮）
  x = 1
  f(2)
end
julia> foo(10)
13
```
在交互式模式下，可以假想有一层作用域块包在任何输入之外，类似于全局作用域：
```
julia> for i = 1:1; y = 10; end

julia> y
ERROR: y not defined

julia> y = 0    #由于会话的作用域类似于全局作用域，因此不必声明gloabl
0
julia> for i = 1:1; y = 10; end
julia> y
10
```
使用以下的语法形式，可以将多个变量声明为全局变量:
```
function foo()
    global x=1, y="bar", z=3
end

julia> foo()
3
julia> x
1
```
- let 语句提供了另一种引入变量的方法。 let 语句每次运行都会声明新变量。 let 语法接受**由逗号隔开**的赋值语句或者变量名：
```
let var1 = value1, var2, var3 = value3
    code
end
```
由于 begin 块并不引入新作用域块，使用 let 来引入新作用域块是很有用的：
```
julia> begin
         local x = 1
         begin
           local x = 2
         end
         x
       end
ERROR: syntax: local "x" declared twice

julia> begin
         local x = 1
         let
           local x = 2
         end
         x
       end
1
```
#### [For 循环及 Comprehensions](http://julia-zh-cn.readthedocs.io/zh_CN/latest/manual/variables-and-scoping/#for-comprehensions)
略
#### 常量
- const 可以声明全局常量和局部常量，最好用它来声明全局常量。全局变量的值（甚至类型）可能随时会改变，编译器很难对其进行优化。如果全局变量不改变的话，可以添加一个 const 声明来解决性能问题。本地变量则不同。编译器能自动推断本地变量是否为常量，所以本地常量的声明不是必要的。
- 特殊的顶层赋值默认为常量，如使用 function 和 type 关键字的赋值。*【这句不懂！！！】*
- 注意 const 仅对变量的绑定有影响；变量有可能被绑定到可变对象（如数组），这个对象仍能被修改。

### 九、类型
Julia 中，如果类型被省略，则值可以是任意类型。添加类型会显著提高性能和系统稳定性。许多Julia程序员可能永远不会觉得有必要去明确地指出类型。然而某些程序会因声明类型变得更清晰，更简单，更迅速及健壮。
Julia 类型系统 的**特性**是，具体类型不能作为具体类型的子类型，所有的具体类型都是最终的，它们可以拥有抽象类型作为父类型。其它**高级特性**有：
- 不区分**对象和非对象值**： Julia 中的所有值都是一个有类型的对象，这个类型属于一个单一、全连通类型图，图中的每个节点都是类型.
- 没有“编译时类型”： 程序运行时仅有其**实际类型**，这在面向对象编程语言中被称为“运行时类型”.
- 值有类型，变量没有类型:  变量仅仅是绑定了值的名字而已.
- 抽象类型和具体类型都可以被其它类型和值参数化: 具体来讲, 参数化可以是符号, 可以是 isbits 返回值为 true 的类型任意值 (本质想是讲, 这些数 像整数或者布尔值一样, 储存形式类似于C中的数据类型或者struct, 并且 没有指向其他数据的指针), 也可以是元组. 如果类型参数不需要被使用或者限制, 可以省略不写.
*【这上面的高级特性无法深刻理解，未来要注意这块！！！！】*
#### 类型声明
:: 运算符可以用来在程序中给表达式和变量附加类型注释。这样做有两个理由：
1. 作为断言，帮助确认程序是否正常运行
2. 给编译器提供额外类型信息，帮助提升性能

- :: 运算符放在表示值的表达式之后时读作“前者是后者的实例”，它用来断言左侧表达式是否为右侧表达式的实例。如果右侧是具体类型，此类型应该是左侧的实例。如果右侧是抽象类型，左侧应是一个具体类型的实例的值，该具体类型是这个抽象类型的子类型。如果类型断言为假，将抛出异常，否则，返回左值.
- 可以在任何表达式的所在位置做类型断言。:: 运算符最常见的用法就是用在函数定义中，来声明操作对象的类型，例如 f(x::Int8) = ... 
```
julia> (1+2)::FloatingPoint
ERROR: type: typeassert: expected FloatingPoint, got Int64

julia> (1+2)::Int
3
```
:: 运算符跟在表达式上下文中的变量名后时，它声明变量应该是某个类型，有点儿类似于C等静态语言中的类型声明。赋给这个变量的值会被 convert 函数转换为所声明的类型;这个特性用于避免性能陷阱，即给一个变量赋值时意外更改了类型。
```
julia> function foo()
         x::Int8 = 1000
         x
       end
foo (generic function with 1 method)

julia> foo()
-24
julia> typeof(ans)
Int8
```
“声明”仅发生在特定的上下文中：
```
x::Int8        # a variable by itself
local x::Int8  # in a local declaration
x::Int8 = 10   # as the left-hand side of an assignment
```
#### 抽象类型
#### 位类型
#### 复合类型
#### 不可变复合类型
#### 被声明类型
#### 多元组类型
#### 类型共用体
#### 参数化类型
#### 类型别名
#### 类型运算


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

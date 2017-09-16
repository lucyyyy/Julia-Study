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
函数调用提供**apply()**方法 *【虽然我不知道这个方法有啥好】*
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



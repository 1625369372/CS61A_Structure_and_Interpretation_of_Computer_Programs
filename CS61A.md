# CS61A: Structure and Interpretation of Computer Programs

## 1. Abstractions of Functions

赋值(Assignment)是一种简单的抽象手段,它将名称与值进行绑定。而函数定义(Function definition)则是一种更加强大的抽象手段,它将名称与表达式进行绑定。

### 函数定义

```python
>>> from operator import add, mul  
>>> add  
<built-in function add>  
>>> mul  
<built-in function mul>
>>> def square(x):
... return mul(x, x)
>>> square  
<function square at 0x1003c1bf8>  
>>> square(11)  
121 
>>> square(add(3,4)) 
49  
>>> square(square(3))  
81  
>>> def sum_squares(x, y):  
... return square(x) + square(y)  
>>> sum_squares (3, 4)  
25
```

1. 用`name(format parameters)`的形式创建一个函数
2. 将该函数的主体设为首行之后缩进的所有内容
3. 在当前框架中将`name`绑定到那个函数

### 解释器的执行过程和环境图

![[attachments/Pasted image 20240513183907.png]]

这张图片展示了一个解释器(interpreter)的执行过程和环境图(environment diagrams)。

左侧的代码部分显示了几个语句(statements)和表达式(expressions),箭头指示了执行顺序。

右侧的框架(frames)部分表明每个名称(name)都绑定到一个值(value)。同一个名称在一个框架内不能重复。

### 赋值规律

给变量`a`赋值，再给`b`赋值$f(a)$，如果在之后的行中修改`a`的定义和赋值，`b`不会有任何改变。以下是代码实例：

```python
>>> from math import pi
>>> radius = 10 
>>> radius 
10 
>>> 2 * radius 
20 
>>> area, circ = pi * radius * radius, 2 * pi * radius 
>>> area 
314.1592653589793 
>>> circ
62.83185307179586 
>>> radius = 20 
>>> area 
314.1592653589793
```

想要一个变量的值随着另外一个变量的变化而变化则需要使用函数。以下是代码实例：

```python
>>> radius  
20  
>>> area  
314.1592653589793  
>>> def area():  
... return pi * radius * radius  
... 
>>> area  
<function area at 0x102982400>  
>>> area()
1256.6370614359173  
>>> pi * 20 * 20  
1256.6370614359173  
>>> radius = 10  
>>> area()  
314.1592653589793  
>>> radius = 1  
>>> area()  
3.141592653589793  
```

python中一个变量可以被赋值或者定义为函数，非常自由。

内置函数也可以被赋值为其他变量或者定义为其他函数。以下是代码实例：

```python
>>> max(1, 2, 3)  
3  
>>> f = max  
>>> f  
<built-in function max>  
>>> max  
<built-in function max>  
>>> f(1, 2, 3)  
3  
>>> max = 7  
>>> f(1, 2, 3)  
3  
>>> f(1, 2, max)  
7  
>>> max  
7  
>>> max = f  
>>> f  
<built-in function max>  
>>> max  
<built-in function max>  
>>> max(1, 2, 3)  
3  
```

如下图所示，没有被赋值给变量的计算式只是被临时存储。
Python 的垃圾回收机制会自动管理内存，当表达式求值完成且没有变量持有其引用时，结果会很快被垃圾回，虽然结果短时间内存在于内存中，但并不会长时间占用内存。

![[attachments/Pasted image 20240523202519.png]]

### 多重赋值

![[attachments/Pasted image 20240523200337.png]]

如以上的示例，Python 会在做实际赋值之前先计算好右边所有表达式的值，然后再同时赋值给左边的变量
### 全局变量和局部变量

![[attachments/Pasted image 20240515133756.png]]
上图展示了python中全局变量和局部变量的区别，全局变量在global frame中生效，而局部变量只在当前函数frame中生效。

![[attachments/Pasted image 20240515134342.png]]
如上述例子所示，如果在当前frame中没有定义局部变量，那么它会被全局变量赋值。
即是current frame中局部变量的优先级高于全局变量，若没有定义局部变量则默认赋为全局变量的值。

以上也是一个展示局部局部变量和全局变量的不同作用域的例子：
![[attachments/Pasted image 20240523202927.png]]

### python在命令行换行

在命令行的 Python 交互式解释器中,可以使用以下三种方式来换行:

1. 使用反斜杠 `\`:
   在行末添加反斜杠 `\`,然后按下回车键,可以将代码续行到下一行。Python 解释器会自动忽略行末的反斜杠和换行符,将下一行的代码与当前行连接起来。

   示例:
   ```python
   >>> result = 1 + 2 + 3 + \
   ...           4 + 5 + 6
   >>> result
   21
   ```

2. 使用括号 `()`, `[]`, `{}`:
   当代码行以左括号 `(`, `[` 或 `{` 开头时,Python 解释器会自动将代码续行,直到遇到相应的右括号 `)`, `]` 或 `}` 为止。在括号内部,可以直接按下回车键进行换行,无需使用反斜杠。

   示例:
   ```python
   >>> my_list = [
   ...     1, 2, 3,
   ...     4, 5, 6,
   ...     7, 8, 9
   ... ]
   >>> my_list
   [1, 2, 3, 4, 5, 6, 7, 8, 9]
   ```

3. 使用三引号 `'''` 或 `"""`:
   使用三个单引号 `'''` 或三个双引号 `"""` 可以创建多行字符串。在三引号内部,可以直接按下回车键进行换行,无需使用反斜杠。

   示例:
   ```python
   >>> message = '''
   ... This is a
   ... multiline string
   ... '''
   >>> print(message)
   
   This is a
   multiline string
   ```

### bash路径中存在空格

bash输入的路径中存在空格时需要使用引号`""`或者`\`来进行转义

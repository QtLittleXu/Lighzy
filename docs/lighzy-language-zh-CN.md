# Lighzy 文档

## 关键字

关键字属于 Lighzy 中特有的标识符，其它标识符的命名都不能与关键字发生冲突，下列是 Lighzy 的所有关键字：
`let, fun, true, false, if, else, return, while, null`

## 环境

Lighzy 中的所有值都会存储在一个环境当中，每个包含块语句的代码都会定义一个新的环境同时包含外部环境。内部环境可以访问外部环境，反之则不行，在同一个环境中不能包含相同的两个标识符。Lighzy 默认提供了一个顶层环境，可以在没有入口函数的情况下执行代码。

### 顶层环境

```swift
// There are two variables in the top-level environment
let a = 11
let b = "Hello world!"

println(a)
```

### 新环境

```swift
let add = fun(a, b)
{
    let i = 0
    while (i < 10)
    {
        let copy = 0    // Declare a variable in an environment for the while statement
        copy = i        // Can access the variable inside
    }
    copy = 11           // Cannot access it

    let result = a + b  // Declare a variable in an environment for the function
    return result
}

println(result)     // Cannot access result outside
```

## 数据类型

Lighzy 中的所有值都有其类型且一旦声明就不可改变，一下是 Lighzy 支持的数据类型：

- Integer：64 位有符号整型
- Float: 64 位双精度浮点数
- Bool：1 位布尔型
- String：长度不定，字符串型
- Array：长度不定，数组型
- Function：长度不定，函数型
- Null：空类型

## 语句

Lighzy 包含一些没有返回值的代码，这里称之为语句，以下是 Lighzy 的所有语句：

### 表达式语句

包含一个表达式的语句，所有语句块的返回值都是语句块最后一个表达式语句的值。

**语法：**`<expression>`

```swift
// eg.
let add = fun(a, b)
{
    a + b   // The return value is the value of last expression
}
```

### let 语句

声明一个不可变变量，必须初始化，类型可根据初始值推导。

**语法：**`let <identifier>: <type> = <value>`

```swift

// eg.
let a = 123
let b = test
let c: string = "Hello world!"

let c               // Error: immutable variable must be initialized
a = 1               // Error: cannot change immutable variable
let t: int = "text" // Error: variable type mismatch
```

### var 语句

声明一个可变变量，如有初始值可根据初始值推导变量类型，否则必须显式指定类型。

**语法：**`var <identifier>: <type> = <value>`

```swift
// eg.
var a = 11
a = 20
var isReal: bool

var str: string = 1 // Error: variable type mismatch
var num             // Error: variable type must be specified explicitly if not initial value
```

### return 语句

用于在函数中返回一个值，如果 `<value>` 为 `null` 表示无返回值。

**语法：**

```swift
let test = fun()
{
    return <value>
}

// eg.
let add = fun(a, b)
{
    return a + b
}

let puts = fun(str)
{
    print(str)
    return null     // No return value
}
```

### while 语句

创建一个循环体，当条件成立时再次执行循环体。

**语法：**

```swift
while (<condition>)
{
    // Do something here
    <statements>
}

// eg.
var count = 10
while (count != 0)
{
    println("Hello lighzy!")
    --count;
}
```

## 表达式

Lighzy 中包含一些有返回值的代码，这里称之为表达式，以下是 Lighzy 的所有表达式：

### 前缀表达式

如果前缀表达式操作数类型不符，则会发生错误。

**语法：**`<prefix> + <operand>`

```swift
// eg.
-12         // Negative prefix
--index     // Decrement prefix
++i         // Increment prefix

++"abcd"    // Error: increment operand type error
```

### 中缀表达式

如果前缀表达式操作数类型不符，则会发生错误。

**语法：**`<operand> + <infix> + <operand>`

```swift
// eg.
1 + 1       // Plus infix
2 != 3      // Logical infix

false == 12 // Error: infix operand type mismatch
```

### 函数表达式

用于返回一个定义的函数，Lighzy 中定义函数的操作就是用 let 语句存储函数表达式完成的。除非特殊需要，不建议使用 var 语句存储函数，因为它可以改变内部存储的值，大多数语言都希望用户不要改变存储的函数。函数参数可以拥有默认值，以便调用时不用全部赋值，默认参数必须全部定义在参数列表的末尾，否则解释器不知道如何赋值参数。

**语法：**

```swift
fun(<arguments>)
{
    // Do something here
    <statements>
}

// eg.
let add = fun(a, b)
{
    return a + b
}

let puts = fun(str)
{
    println(str)
}

let add = fun(a = 0, b = 0)
{
    a + b
}

// Error: invalid declaration for default arguments
let sub = fun(a = 0, b)
{
    a - b
}
```

### 调用表达式

调用一个函数表达式，在没有默认参数的情况下，新形参数量与实参数量不符，则会发生错误。

**语法：**`<function>(<arguments>)`

```swift
// eg.
let add = fun(a, b)
{
    return a + b
}

let sum = add(1, 2)     // sum: 3
let sum = add(21, 2, 2) // Error: invalid arguments: expected the number of them to be 2, but got 3

// Default arguments
let add = fun(a = 0, b = 0)
{
    a + b
}

add()       // Return 0
add(12)     // Return 12
add(2, 3)   // Return 5
```

### 赋值表达式

赋值给一个使用 var 语句声明的变量，如类型不匹配和尝试赋值一个 let 语句声明变量，则会发生错误。

**语法：**

```swift
<variable> = <value>

var num = 12
num = 2

num = "de"  // Error: assignment operand type error
let a = 11
a = 1       // Error: cannot change immutable variable
```

### 判断表达式

不同于大部分语言，Lighzy 的判断操作为一个表达式，因此具有返回值既最后一个表达式语句的值。当判断条件为真，将会执行 `if` 语句块，否则执行 `else` 语句块，`else` 语句块可以省略。

**语法：**

```swift
if (<condition>)
{
    // Do something when condition is true
    <statements>
}
else
{
    // Do something when condition is false
    <statement>
}

// eg.
let status = false
let result = if (status)
{
    true
}
// result: true

let value = 11
if (value == 11)
{
    println("value: 11")
}
else
{
    println("value: i dont know")
}
```

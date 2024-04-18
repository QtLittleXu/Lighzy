# Lighzy Documentation

## Keywords

Keywords are special identifiers in Lighzy. The naming of identifiers except keywords cannot conflict with them. Here are all keywords in Lighzy:
`let, fun, true, false, if, else, return, while, null`

## Environments

All values in Lighzy are stored in environments. Each piece of code including Block Statement declares a new environment and it contains the inner one. The inner environment can access the outer one but not vice versa. In a environment, there cannot be tow same identifiers. At the end, Lighzy provides a top-level environment to execute code without the entry function.

### Top-level Environment

```swift
// There are two variables in the top-level environment
let a = 11
let b = "Hello world!"

println(a)
```

### New Environment

```swift
let add = fun(a, b)
{
    let i = 0
    while (i < 10)
    {
        let copy = 0    // Declared a variable in the environment of the while statement
        copy = i        // Can access the variable inside
    }
    copy = 11           // Cannot access it

    let result = a + b  // Declared a variable in the environment of the function
    return result
}

println(result)     // Cannot access result outside
```

## Data Types

The value in Lighzy has its own type and it cannot be changed if it has declared. Here are all data types Lighzy supported:

- Integer：64-bit signed integer
- Float: 64-bit double float
- Bool：1-bit boolean
- String：string of variable length
- Array：array of variable length
- Function：function of variable length
- Null：nil

## Statements

The code that do not have the return value in Lighzy is called Statement. Here are all statements:

### Expression Statement

A statement which contains an expression. The return value of a code block is the value of the last expression statement.

**Syntax:** `<expression>`

```swift
// eg.
let add = fun(a, b)
{
    a + b   // The return value is the value of the last expression
}
```

### Let Statement

Used to declare an immutable variable and it has to be initialized. The type can be inferred from the initial value.

**Syntax:** `let <identifier>: <type> = <value>`

```swift
// eg.
let a = 123
let b = test
let c: string = "Hello world!"

let c               // Error: immutable variable must be initialized
a = 1               // Error: cannot change immutable variable
let t: int = "text" // Error: variable type mismatched
```

### Var Statement

Used to declare a mutable variable. The type could be inferred from the initial value if it exists. Otherwise must declare the type explicitly.

**Syntax:** `var <identifier>: <type>(optional) = <value>(optional)`

```swift
// eg.
var a = 11
a = 20
var isReal: bool

var str: string = 1 // Error: variable type mismatched
var num             // Error: the type must be declared explicitly if the initial value do not exists
```

### Return Statement

Used to return a value in a function. `value` is `null` indicating no return value.

**Syntax:**

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

### While Statement

Used to create a loop code block. Loop statements will rerun if the condition is true.

**Syntax:**

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

## Expressions

The code that returns a value in Lighzy is called Expression. The following are all expressions:

### Prefix Expression

Contains a prefix and a operand. It is an error that the type of the operand is mismatched with the prefix.

**Syntax:** `<prefix> + <operand>`

```swift
// eg.
-12         // Negative prefix
--index     // Decrement prefix
++i         // Increment prefix

++"abcd"    // Error: the increment prefix is mismatched with the string
```

### Infix Expression

Contains a infix and two operands. It is an error that types of operands are mismatched with the infix.

**Syntax:** `<operand> + <infix> + <operand>`

```swift
// eg.
1 + 1       // Plus infix
2 != 3      // Logical infix

false == 12 // Error: the infix is mismatched with operands
```

### Function Expression

Used to return a defined function. Defining a function in Lighzy is to use let expression to store function expression. It is not recommended to store function expression into var statement except specific need, because most languages want not to let the user to change stored functions. Arguments of functions can have initial values so that there is no need to assign all arguments. Default arguments have to be declared at the end of the parameter list otherwise the interpreter does not understand how to assign arguments.

**Syntax:**

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

### Calling Expression

Used to call function expression. It is wrong that formal arguments are not matched with actual arguments with out default arguments.

**Syntax:** `<function>(<arguments>)`

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

### Assignment Expression

Used to assign a variable declared with var statement. They are wrong that types are mismatched and attempt to assign a variable declared with let statement.

**Syntax:**

```swift
<variable> = <value>

var num = 12
num = 2

num = "de"  // Error: assignment operand type error
let a = 11
a = 1       // Error: cannot change immutable variable
```

### If Expression

Unlike most languages, the condition in Lighzy is an expression. So there is a return value which the value of the last expression. If the condition is true, the `if` code block is about to be executed otherwise the `else` one. The `else` code block is optional.

**Syntax:**

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
    println("value: i don't know")
}
```

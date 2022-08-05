# Kotlin (Work In Progress)
Kotlin is the statically typed, multiplatform programming language that support type inference.

Lets understand each of these terms above:
* **Statically typed**: It means that in koltin the type of the variable is determined at compile only.
* **Multi-Plaform**: It means that kotlin can be used to write computer softwares that can run on multiple plafroms.
* **Type Inference**: It means that type of the variable can be automatically determined by the kotlin compiler.

# Table of Contents:

In this page we will learn about the following topics of Kotlin language:

1. [Data Types](#data-types-and-basics)
2. [Conditional Logics](#conditional-logics)
3. [Loops](#loops)
4. [Collections](#collections)
5. [Object Oriented Programming](#object-oriented-programming)
6. [Asynchronous programming](#asynchronous-programming)
7. [Coroutines](#coroutines)
8. [Annotations](#annotations)
9. [Reflections](#reflections)
10. [Exception Handling](#exception-handling)
11. [Kotlin Java Interoperability](#kotlin-java-interoperability)


## Data Types and basics
Before understanding data types we will first try to understand what are **literals**. 

**Literals:** In kotlin or any other programming language we operate on numbers, character and strings. These are called **literals** the very basic entity of a language. Any thing that we do involve these **literals**.\
So, there are 3 types of literals:
* Numbers: These are used for repesent quantity. Eg: 1, 1_2_3, 10000, 10_000, 10___000
* Character: A single digit, word or special character. They are enclosed inside single quotes. Eg: 'a', '#', '1', '1a'(invalid character because it has 2 entities a number and a word).
* String: Group of one or more character enclosed inside double quotes. Eg: "A", "A a", "A_#"

Data type for Numbers:

| Data type | Size(bits) | Min. Value | Max. Value |
| --- | ----------- | --------- | ---------- |
| Byte | 8 | -2<sup>7</sup> | 2<sup>7</sup> - 1 |
| Short | 16 | -2<sup>15</sup> | 2<sup>15</sup> - 1 |
| Int | 32 | -2<sup>31</sup> | 2<sup>31</sup> - 1 |
| Long | 64 | -2<sup>63</sup> | 2<sup>63</sup> - 1 |

In kotlin we represent binary numbers as `0b10` and hexadecimal numbers as `0xFA`. There is no octals in kotlin.

To store these data types we use `variables`.\
A variable has a name called `identifier`.\
This `identifier` must be different for different variables.\
We can access variables value with the help of these identifiers.

To declare a variable we use two different keywords `val` and `var`.\
`val` is used to define a variable whose values shouldn't change.\
`var` is used to define a normal variable, that can change with the program execution.

We should try to use `val` over `var` for variable declaration because a changing variable decreases the redability of code.

We use tags to define certain data types like: `float`, `long`.\
Eg: `val floatValue = 1.314f`, `var longValue = 123_456_789L; longValue = 100_000_000_000L`

For normal variables the data type of the value assigned to them must be same as at the time of initialisation of the varaible.\
Eg:
```
var mutableVariable = 100
mutableVariable = "100" //this line will give error
```
but, the following will work fine: 
```
var longValue = 100L
longValue = 100_000_000_000L
```

`val` variables must not be misunderstood as immutable. It means that the value of the variable once assigned can never be changed. However the internal state of the value of the variable can change.\
Eg:
```
val listOne = mutableListOf(1,2,3)
listOne = mutableListOf(1,2,3,4) // this line will give error
listOne.add(4) //this line works fine
```

We must assign some value to `val` variables before accessing them. If we try to acess them when they are not assigned any value we get error.
```
fun main() {
    val value:Int
    println(value)// this line gives error
}
```

So, `val` is not constant, but if we want to define constants, we use `const` keyword for that.\
`const` variables must have primitive data types as values.\
And they must be defined at the top level outside of functions or class scope or inside `companion objects`.
```
const val temp1="This is constant variable1"
class Temp(){
    companion object{
        const val temp2="This is constant variable2"
    }
}

fun main() {
    println(Temp.temp2)
    println(temp1)
}

```

Variable declaration in kotlin can be done in two ways:
1. `val/var identifier = Initialization value` 
2. `val/var identifier:Type = Initialization value`

For the first type of declaration the type of the variable is determined by kotlin itself. This feature of kotlin is called **type inference**.\
For this to be valid variable declaration we must initialize the variable at the time of defining it.\
Eg:
```
var integer = 1 // is a valid variable declaration

//vs

var str
str = "String" // is an invalid variable declaration and it will through error
```

For the second type of variable declaration the `Type` of the variable must be in captial letters and in this declaration initialization of variable can be done in future
```
var integer:Int = 1

//vs

var str:String
str = "String"
//both are valid variable declaration
```

Here `//` between the code blocks denote comments. Comments are used to explain the code to developer and tell the compiler to not execute a specific block of the program. There are 3 ways in which we define comments in kotlin:
1. End of line comments (`// comments`): kotlin compiler ignores whatever is written after `//` until the end of line
2. Multi line comments (`/** comments **/`): kotlin complier ignores whatever is written in between `/**` and `**/`
3. Documentation comment (`/** Docs string */`): Koltin complier treat this type of comment as a docstring. And hence provides special keyword like  `@return`, `@params` and so on, for providing better explanation to different parts of code.


</br>
</br>
Finally to recap:

| Data type | Size(bits) | Min. Value | Max. Value |
| --- | ----------- | --------- | ---------- |
| Byte | 8 | -2<sup>7</sup> | 2<sup>7</sup> - 1 |
| Short | 16 | -2<sup>15</sup> | 2<sup>15</sup> - 1 |
| Int | 32 | -2<sup>31</sup> | 2<sup>31</sup> - 1 |
| Long | 64 | -2<sup>63</sup> | 2<sup>63</sup> - 1 |
| Float | 32 | upto 6-7 decimal places | upto 6-7 decimal places |
| Double | 64 | upto 14-16 decimal places | upto 14-16 decimal places |
| Char | 16 | - | - |

`Boolean: true/false`

**To know the maximum/minimum values of numbers:**\
`Int.MAX_VALUE`, `Long.MIN_VALUE`, similarly for other types.\
**To know of numbers in bits/bytes format:**\
`Int.SIZE_BYTES` or `Int.SIZE_BITS`

**STDIN/STDOUT:**\
To take input from the STDIN we use following methods:
1. `readLine()!!`
2. `readln()`
3. `Scanner`

**`readLine()!!`:** Is used to read one non-nullable line from the STDIN\
**`readln()`:** Works similar to `readLine()` but was introduced in kotlin 1.6 and later.\
**`Scanner`:** We can also use javas scanner class in kotlin\
To assign value to multiple variables at the same time we use:\
`val (a,b,c) = readln().split(" ")`

To use `scanner` class following is the syntax:
```
Scanner scanner = Scanner(System.`in`)
val singleStringReading = scanner.next() // reads a single word from the STDIN line and places the cursor after the scanned string
val integerReading = scanner.nextInt() // reads a number from the STDIN line and places the cursor after the scanned number
val readLine = scanner.nextLine() // reads the whole line from the STDIN line and places the cursor on the next line
```
Similarly there are other reading functions available in `Scanner` class that can be used based on requirement.

For **data type conversion** following are the available methods:
`toString()`, `toInt()`, `toFloat()`, `toDouble()`, `toByte()`,`toShort()`, `toLong()`, `toBoolean()`.\
Data type conversion from larger type like `Long` to smaller type like `Int` may lead to `type overflow`. So we need to keep this in mind while doing type conversions.\
If we try to convert `String` to `Boolean` we will get `true` only if the string is having case insensitive "true". Else every other thing is False.

## Conditional Logics

## Loops

## Collections

## Object Oriented Programming

## Asynchronous Programming

## Coroutines

## Annotations

## Reflections

## Exception Handling

## Kotlin Java Interoperability

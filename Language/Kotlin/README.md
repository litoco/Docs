# Kotlin (Work In Progress)
Kotlin is the statically typed, multiplatform programming language that support type inference.

Lets understand each of these terms above:
* **Statically typed**: It means that in koltin the type of the variable is determined at compile only.
* **Multi-Plaform**: It means that kotlin can be used to write computer softwares that can run on multiple plafroms.
* **Type Inference**: It means that type of the variable can be automatically determined by the kotlin compiler.

# Table of Contents:

In this page we will learn about the following topics of Kotlin language:

1. [Data Types](#data-types)
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


## Data Types
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

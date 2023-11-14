---
layout: page
title: Introduction to Kotlin
permalink: /kotlin-intro/
---

# Introduction to Kotlin

Throughout this book we have been using the Java programming language to create our apps but we can also create them using Kotlin. In this chapter we will go on a quick tour of the Kotlin programming language.

Kotlin is a language that is created by JetBrains that can be used to create server side and Android applications. It is similar to Java in that it is compiled to bytecode that can run on the JVM (Java Virtual Machine). It can also compile to JavaScript or native code but this is beyond the scope of this book.

Here are some of the key features of Kotlin.

## Variables

The main thing to remember about Kotlin variables is that they can either be a `val` or a `var`. A variable that starts with the `var` keyword is said to be mutable and can be changed later on in the program. A variable the starts with the `val` keyword is immutable and its value, once assigned, cannot be changed.

Here are some of the built in data types the Kotlin has.

Int - holds whole numbers like `0`, `1`, `4`

Boolean - holds the value `true` or `false`

Character - holds characters like `'a'`, `'B'`

String - holds textual values like `"Hello"`

Double - holds floating point numbers like `0.1`, `10.89`


## Functions

Think of functions as a way to package functionality that you can then use in different sections in your application. For instance, in the following code snippet we have two functions `message` and `feetToInches`. 

Let's look at the first function, `message`. Its signature, `fun message(): String{...}`, starts with the keyword `fun`. This means that it is a function. It has an empty parenthesis `()`. This means that this function does not take in any values. We then have the keyword `String` after the colon `:`. This means that the function is expected to return a value of type `String`. In our example it will return the `String` "feet to inches is".

In our second function `feetToInches` we have the words `feet: Int` inside the parenthesis `()` and we have the keyword `Int` after the colon `:`. This means that our function will accept a user defined variable feet, of type `Int` and return an `Int`. We can have zero are more parameters inside the parenthesis `()`. If our function accepts more than one parameter we simple separate them with a comma.

We have packaged our functionalities into two functions. One function returns a simple message and the other converts feet into inches. We can now use them anywhere in our program. Let's use it to convert 6 feet into inches. We declare our own variables called `feet`, `msg` and `conv` to hold the number 6, our message and the conversion respectively. The line `val feet: Int = 6` will store the number 6. The line `val msg: String = message()` will call our first function and store the message in `msg`. The line `val conv: Int = feetToInches(6)` will call our second function with the number 6 and return the conversion and store it in `conv`.

Finally we concatenate (join together) all our variables to print out the final results with the line `println ("$feet $msg $conv")`.

~~~
fun message(): String {
    return "feet to inches is"
}

fun feetToInches(feet: Int): Int {
    val ans: Int = feet * 12

    return ans
}

val feet: Int = 6
val msg: String = message()
val conv: Int = feetToInches(6)

println ("$feet $msg $conv")
~~~


## Control Statements

Just like in Java, Kotlin programs are executed by the computer from top to bottom. However, if you want to jump to another section or loop around a section then we use control statements. We're going to take a look at `if` and `when` statements as well as `for` and `while` loops.


### If-Else Statement

An `if` statement will execute or skip a block of code depending on
whether a given condition is true or not.

In the following segment, the section that prints `Today is Monday` to the screen will be executed if the program is run on a Monday. However if it run on any other day `Today is not Monday` will be printed.

~~~
import java.time.LocalDate

var date: LocalDate = LocalDate.now() //gets today's date
var today: Int = date.dayOfWeek.value //gets the day of the week from today's date

if (today == 1){
    println("Today is Monday")
} else {
    println("Today is not Monday")
}
~~~

### When Statement

A when statement is a conditional statement. Your program will go through each line of the statement and will execute it if that particular condition is true. The branch that is executed depends on the value of `today`.

~~~
import java.time.LocalDate

var date: LocalDate = LocalDate.now() //gets today's date
var today: Int = date.dayOfWeek.value //gets the day of the week from today's date

when (today){
    1 -> println("Today is Monday")
    2 -> println("Today is Tuesday")
    3 -> println("Today is Wednesday")
    4 -> println("Today is Thursday")
    5 -> println("Today is Friday")
    else -> println("Today is a weekend!")
~~~

### While loop

Another important control statement is the while loop. We use it to execute a block of code over and over again while a condition remains true. This is illustrated in the subsequent code snippet. In the snippet the value of the counter `i` will continue to be displayed while it is less than the number `5`. You will also notice that we increment the counter `i++` in the loop. 

~~~
var i: Int = 0
while (i < 5){
    println(i)
    i++
}
~~~

The output of our snippet will be,

~~~
0
1
2
3
4
~~~

### For loop

Another type of loop is the for loop. We use a for loop when we want to iterate over a list of items while executing a block of code. In our example we iterate over the numbers `0` to `3`. We display the number during each iteration.

~~~
for (i in 0 .. 3){
    println(i)
}
~~~

## Null Safety

In programming, `null` means that something has no value. Put in another way a variable with no value is `null`. Trying to access or use a variable with a `null` value will generally result in an error. To prevent this, all variables in Kotlin cannot be `null` unless you explicitly say so. 

Let's look at this example. If you try running the code below, you will get an `Null can not be a value of a non-null type String` error.

~~~
var message: String = "Welcome to Kotlin"
message = null //error
~~~

Now if you want to forgo this protection and assign null to a variable, you'll have to add the `?` operator to the type name like so;

~~~
var message: String? = null
println(message)
~~~

If `message` was `null`, like in the snippet below, and you tried to use it you would get a compilation error. 

~~~
var message: String? = null
println(message.length) //error
~~~



To access `message` safely you would have to use a **safe** or **non-null asserted call**.

### Safe Call

A safe call uses the (`?.`) operator.

~~~
var message: String? = null
println(message?.length) //safe call
~~~

### Non-null Asserted Call

We only use the (`!!.`) operator when you are sure that a variable is not `null`. If the variable is `null` your program will throw a `NullPointerException` which we do not want.

~~~
var message: String? = "Welcome to Kotlin"
println(message!!.length)
~~~

### Elvis Operator

In the example below we use the Elvis operator (`?:`). It can be used as a shorthand for dealing with `null` values. In our example we get the length of `message` however is `message` is `null` return `-1`. Again, in the second example if `message2` is `null` then `-1` will be returned and the length otherwise.  

~~~
var message : String? = "Welcome to Kotlin"
val len: Int = message?.length ?: -1
println(len) // returns length

var message2 : String? = null
val size: Int = message2?.length ?: -1
println(size) // returns -1
~~~

## Classes and Objects

As we learned earlier a class is a template for creating objects and the creation of the object is called "instantiation". In our example, we instantiate the method `obj` using the `var obj : Foo = Foo()` line. In order words `obj` is an instance of the `Foo` class. We then call the `myMethod` function using the `var obj : Foo = Foo()` line to print "Hello World!" to the screen.

~~~
class Foo {
    fun myMethod() {
        println("Hello World!")
    }
}

var obj : Foo = Foo()
obj.myMethod()
~~~

## Lambda Function

Previously we talked about functions. We are now going to talk about a special kind of function called a lambda function. Lambda functions are functions that can be stored in a variable. We can then carry the function around in our program using this variable. The function can then be executed whenever we want results. 

In this example we have a lambda function `{x: Int -> x * 12}` which takes a variable of type `Int` and multiplies it by 12 to produce a result. We then immediately invoke this lambda function by passing it the value of `feet` in the parenthesis `()`.

~~~
val feet: Int = 6;
val inches = {x: Int -> x * 12}(feet)

println("$feet feet is $inches inches" )
~~~

## Running your Kotlin code

We are going to use the Scratch Pad to run our Kotlin code by following these steps.

1. File -> New -> Scratch File

2. Select Kotlin in the "New Scratch File" drop-down

3. You can now type your code and then press the Run button once you're ready to run your program

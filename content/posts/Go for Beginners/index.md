---
title: "GO for Beginners"
date: 2021-05-18
description: "GO"
tags: ["GO","Golang"]
type: post
weight: 20
showTableOfContents: true
---

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

![img01](images/01.webp)

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

## Getting Started with Go: Syntax and Your First Program

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Go Syntax Basics**
In `Go`, we use statements to perform actions. These are usually terminated by a new line or a semicolon (`;`). The semicolon in the end of the statement is optional and used rarely. `Go` uses curly braces (`{ }`) to group statements into blocks.

Take a look at this simple `Go` syntax example:
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go World!")
}
```

In this example, `package main` specifies that it's the main entry point of the `Go` application. `import "fmt"` instructs `Go` to use the `fmt` library for formatted I/O operations, and in the `main` function, we print `"Hello, Go World!"`. Though you may not fully understand this code just yet, we will explore each part of it step-by-step in forthcoming lessons.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Say Hello to Comments**

Similar to other programming languages, `Go` supports two kinds of comments: single-line comments and multi-line comments. Single-line comments begin with `//`, while multi-line comments are surrounded by `/* */`. Comments provide documentation and critical reference points within the code and don't affect the program execution. They are meant to make the codebase understandable and easy to work with.

Here's an example of how you can use comments in your `Go` programs:
```go
package main  // Single-line comment

import "fmt"

/* This is a multi-line comment in Go
   It spans multiple lines
   Useful for longer descriptions and notes */

func main() {  
    fmt.Println("Hello, Go World!") // This line prints "Hello, Go World!"
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Build Your First Go Program**

Now that we've covered the basics, let's delve deeper into the first Go program you'll write! Here is a simple Go program we have already encountered:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go World!")
}
```
Let's understand each part of the program:

- `package main`: This line declares the name of the package that this file resides in, which in this case is `main`. The `main` package is special in `Go`. It defines an executable program, rather than a library.

- `import "fmt"`: This statement imports another package into the current package. The package `fmt` provides functionalities for formatted input/output.

- `func main() { }:` This line defines the main function of the program, which is executed when we run our `Go` program. Similar to Java's `public static void main(String[] args)`, the main function in `Go` serves as the entry point for the program.

- `fmt.Println("Hello, Go World!")`: This is a function call to `fmt.Println()`, which prints out the passed string to the console.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Understanding Variables and Constants in Go

**Topic Overview and Goal Setting**

Greetings! As we venture into our ***Go Programming Expedition***, we're setting out to understand **Go Variables**, our essential assistants. Similar to coordinates on a map, variables guide our code, endowing it with data and meaning.

In simple terms, a variable in coding is akin to a ticket — a reserved place in memory where a value can be stored. This lesson aims to demystify the concept of Go variables, examining their definition, naming conventions, assignment operations, and the concept of constant variables.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**What are Go Variables?**

Think of Go variables as tickets, each carrying specific data. The following short example illustrates how a variable is defined in Go:
```go
var numOfMountainPeaks int // We declare a variable, similar to buying a ticket
numOfMountainPeaks = 14 // We then assign it a value
fmt.Println(numOfMountainPeaks) // Finally, we validate its contents. It outputs: 14
```

Here, `int` is the data type of the variable (integer), `numOfMountainPeaks` is the variable's name, and `14` is its value. We will delve deeper into data types in the upcoming lesson, so don't fret if the `int` part is somewhat unclear now.
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Alternatively, you can declare and assign the variable in one step like this:
```go
var numOfMountainPeaks = 14 // declaring and assigning the variable at once
fmt.Println(numOfMountainPeaks)
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Or using a short declaration:
```go
numOfMountainPeaks := 14 // creating and assigning the variable with shorthand
fmt.Println(numOfMountainPeaks)
```

To sum up, here are all the ways to initialize a variable:

- Declaring without initializing: `var name int`;

- Declaring and Initializing: `var name = 5`;

- Short Declaring and Initializing: `name := 5`;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Go Naming Conventions**

Just as with correctly labeling a ticket, naming a Go variable requires adherence to certain rules and conventions. These assist us in keeping our code error-free and easily interpreted by others.

Go's variable name rules follow the CamelCase convention: If the variable name contains a single word, all letters should be lowercase. If the variable name comprises multiple words, the first one should be lowercase, and each subsequent one should start with a capital letter. For example, `age`, `weight`, `myAge`, `firstDayOfWeek`.

Special characters and digits are not permitted at the start of variable names.
```go
// Correct variable naming
var myWeight int = 72
var district9Population int = 10000
myAge := 20

// Incorrect variable naming (commented intentionally)
// var 0zero = 0;
// var ?questionMark = 1;
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Assignment Operations in Go**

Assigning in Go involves allocating or updating a variable's value using the `=` operator. This act is analogous to stamping a ticket.
```go
var constellations = 88 // We secure a ticket, label it, and assign a value
fmt.Println(constellations) // We check the content. It outputs: 88

constellations = 77 // We modify the value of the variable
fmt.Println(constellations) // We review the updated content. It outputs: 77
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Go Constants**

While the previous section describes how to change a variable's value, Go also provides a method to define constants — variables that cannot alter their value once assigned. We use the `const` keyword to declare a constant. Constants are generally named using uppercase letters, and words are separated by underscores `_`.

Declaring a value as `const` is a common practice when you know it won't change, thereby enhancing readability, providing safety (avoiding accidental changes), and sometimes improving performance reasons.
```Go
const DAYS_IN_WEEK = 7 // We define a constant, similar to etching a fact on a monument
fmt.Println(DAYS_IN_WEEK) // We examine our immutable fact. It outputs: 7

// DAYS_IN_WEEK = 6; // This will not compile
```
Here, `DAYS_IN_WEEK` serves as a constant, disallowing adjustments once assigned. The value for this variable cannot be changed after its assignment.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Introducing Numerical Data Types

In Go, we use numerical data types to represent numbers. Specifically, in this lesson, we're focusing on `int` and `float64`. The `int` data type represents whole integer numbers, and the `float64` data type signifies decimal numbers — numbers with a decimal point.

The largest value an int can store depends on the system. It's The largest value an int can store depends on the system. It's 
2³² (`2147483647`) on a 32-bit system and 2⁶⁴ (`9223372036854775807`) on a 64-bit system.. Here's an example of using the `int` number:


```go
var daysInWeek int = 7
fmt.Println(daysInWeek)  // This will print: 7

var maxInt64 int = 9223372036854775807
fmt.Println(maxInt64)  // This will print: 9223372036854775807
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Now, let's move on to the `float64` data type. We use `float64` when dealing with numbers that have decimal points, also known as floating-point numbers. It provides a precision of 15–17 digits. Consider the following example:
```go
var pi float64 = 3.1415926
fmt.Println(pi)  // This will print: 3.1415926
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Discovering Boolean and Byte Data Types**

Let's now shift focus to the `bool` and `byte` data types.

The `bool` data type in Go can hold one of two possible values: `true` or `false`. This data type receives extensive use in logical expressions and decision-making. Here's a simple example:
```go
var isEarthRound bool = true
fmt.Println(isEarthRound)  // This will print: true

var isEarthFlat bool = false
fmt.Println(isEarthFlat) // This will print: false
```

The `byte` data type is a special type of integer. Each symbol (character) is associated with some code. With `byte` data type we can store a symbol's code in a variable.Here's how to use it:
```go
var firstLetterOfAlphabet byte = 'A'  // must be surrounded by SINGLE quotes
fmt.Println(firstLetterOfAlphabet) // This will print: 65 – the code for "A"
```
Note that in Go there is no special data type for storing characters. If you want to store a single symbol, store it inside the `string` data type. Let's explore it.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Exploring String Data Type**

You'll find that the `string` data type is as common in Go as there are stars in the cosmos. Go treats `string` as a basic data type and uses it to store a sequence of characters — just a piece of text. The string is always surrounded by double quotes.
```go
var welcome string = "Welcome to Go!"
fmt.Println(welcome) // This will print: Welcome to Go!
```
Interestingly, `string` in Go is immutable. Once a `string` is created, we cannot change its value.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Understanding nil**

As we conclude this journey, we will discuss a very special value: the `nil` value. `nil` means "no value" or "nothing", or "unknown". It's &&&**not equivalent** to an empty string (`""`) or 0. You can't assign `nil` to a regular variable. But you can assign a **pointer** to nil. Pointer is a variable that holds not the value itself, but a link to the value. It effectively points to a specific place in RAM where the required value is stored. We will explore pointers later, by now let's use it to see `nil` in action.

Here's how you assign `nil` to a pointer:
```go
var unknown *string = nil
fmt.Println(unknown)  // This will print: <nil>
```
While `string` is a data type that stores strings, `*strings` is a pointer to a place where we expect to find a string. But in this case, our pointer points nowhere.

Note: As `nil` is nothing, you can't perform any operations on it. You can still print the `nil` variable or reassign it to an actual value, but you can't perform any other operations on it. Attempting to do so will cause an error known as `nil pointer` `dereference`. But no worries, we will cover this in detail in subsequent lessons!

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Understanding Go Comparison Operators: A Dive into Conditional Logic

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Exploring Go Comparison Operators**

Imagine operating a submarine in the deep sea. Here, you determine your routes by evaluating conditions such as the distances to underwater landmarks. These decisions boil down to comparisons, similar to situations encountered in programming. In Go, we use comparison operators to facilitate such rational decision-making.

The Go programming language includes six comparison operators: equal to (`==`), not equal to (`!=`), greater than (`>`), less than(`<`), greater than or equal to (`>=`), and less than or equal to (`<=`). These operators return either true or false, also known as Boolean values.

Consider this comparison of a submarine's speed in relation to an ocean current as an example:
```Go
var submarineSpeed = 30  // speed in knots
var currentSpeed = 20    // speed in knots 
fmt.Println("Is the submarine faster than the ocean current? ", submarineSpeed > currentSpeed)
// Prints: Is the submarine faster than the ocean current? true
```
In the above code, we used the `>` operator to compare the `submarineSpeed` and the `currentSpeed`. The result is true because the `submarineSpeed` is greater than the `currentSpeed`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Exploring `==` and `!=` Operators**

Now, let's delve into the equal to (`==`) and not equal to (`!=`) operators. These become crucial when you need to compare values, such as when comparing the current oxygen level to the desired one:

```Go
var currentOxygenLevel = 70  // current oxygen level in %
var requiredOxygenLevel = 100  // required oxygen level in %

var isOxygenEnough = currentOxygenLevel == requiredOxygenLevel  // this results in 'false'
var isOxygenLow = currentOxygenLevel != requiredOxygenLevel  // this results in 'true'
```
The `==` operator checks whether the `currentOxygenLevel` equals the `requiredOxygenLevel`, yielding a `false` result. Conversely, the `!=` operator verifies their inequality, returning `true`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Exploring `<`, `>`, `<=`, and `>=` Operators**

Next, let's examine the less than (`<`), greater than (`>`), less than or equal to (`<=`), and greater than or equal to (`>=`) operators. These operators are primarily used for numeric data comparisons. Suppose you're surveying two underwater caves and want to determine which one is closer. You can utilize these operators to make an informed decision:
```Go
var distanceToCaveA = 2000 // distance in meters
var distanceToCaveB = 1000 // distance in meters

var isACloser = distanceToCaveA < distanceToCaveB  // this results in 'false'
var isBCloserOrSame = distanceToCaveA >= distanceToCaveB  // this results in 'true'
```
Here, we compare the distances to two underwater caves. The submarine is not closer to cave A, so `isACloser` is `false`. However, the submarine is closer to, or at the same distance from, cave B —`isBCloserOrSame` is `true`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Mastering Arithmetic and Logical Operations in Go

&nbsp;&nbsp;&nbsp;

**Arithmetic Operations Exposed**

Remember that Go's primitive data types include `int` for whole numbers, `float64` for decimal numbers, `bool` for true/false values, and `string` for textual content. Both `int` and `float64` exhibit constraints in their numerical spans, which we'll explore when we discuss overflow later in this lesson.

We can perform arithmetic operations — addition (`+`), subtraction (`-`), multiplication (`*`), division (`/`), and modulus — the remainder of the division (`%`) — on numerical types. Here's how we do it:
```Go
package main
import "fmt"

func main() {
    var a int = 10
    var b int = 2
    fmt.Println(a + b) // Outputs: 12
    fmt.Println(a - b) // Outputs: 8
    fmt.Println(a * b) // Outputs: 20
    fmt.Println(a / b) // Outputs: 5
    fmt.Println(a % b) // Outputs: 0
}
```
Go supports alteration of order using parentheses and provides the modulus (`%`) operation, handy for determining whether numbers are even or odd!

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Logical Operations Demystified**

Logical operators — `&&` (AND), `||` (OR), `!` (NOT) — function as decision-makers in Go, returning `bool` values — `true` or `false`. Here's how we can utilise them with two `bool` variables:
```Go
package main
import "fmt"

func main() {
    fmt.Println(true && true) // true
    fmt.Println(true && false) // false
    fmt.Println(false && true) // false
    fmt.Println(false && false) // false

    fmt.Println(true || true) // true
    fmt.Println(true || false) // true
    fmt.Println(false || true) // true
    fmt.Println(false || false) // false

    fmt.Println(!true) // false
    fmt.Println(!false) // true
}
```
In this case, `&&` yields `true` only if both boolean inputs are `true`, `||` outputs `true` if either of the inputs is `true`, and `!` reverses the boolean value.

However, the primary use of logical operations is with variables. Let's quickly illustrate the basic usage:
```Go
package main
import "fmt"

func main() {
    var speed int = 60
    var minSpeed int = 30
    var maxSpeed int = 70
    // Check if the speed is within the accepted range.
    fmt.Println(speed > minSpeed && speed < maxSpeed); // Prints: true
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Overflow Phenomenon**

The concept of **overflow** explains what happens when we exceed the range allocation of an integer variable. It happens when we attempt to store a value that surpasses the capacity of the variable's type:
```Go
package main
import "fmt"
import "math"

func main() {
    var maxInt int = math.MaxInt32 // the maximum integer, equivalent to 2^31 - 1
    var overflow int = maxInt + 1 // causes an overflow, there's no integer after the maximal one.
    fmt.Println(overflow) // Prints: -2147483648, which is -2^31 - the minimum integer number
}
```
Here, `maxInt` is the largest integer value `int` can encapsulate. Note that we need to import `"math"` to use the `math.MaxInt32` variable. When we increment it by one, it 'overflows' to the lowest possible integer value! This reminds us that integer values are "cyclic" in nature.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example: 
```go
package main

import (
    "fmt"
    "strconv"
)

func main() {
    // Temperature reading in Kelvin
    tempReading := 15000.95

    // Convert the temperature from a float64 to a string
    tempString := strconv.FormatFloat(tempReading, 'f', -1, 64)

    // Output the temperature as string
    fmt.Println("Log entry - Temperature reading:", tempString, "K")
}
```
* -1 → smart formatting

* 2 → exactly 2 decimal places

* -2 → ❌ compilation error
```
strconv.FormatFloat(f float64, fmt byte, prec int, bitSize int) string
```

| 'f' |    Asay number |    123.45 |
|-----|----------------|-----------|
|“e”  |  Exponential   | 1.234500e+02 |
|“E”   | Exponential (E) |  1.234500E+02 |
|“g”   | Auto (short)  |  123.45  |
|'G'   | Auto (E)   | 1.2345E+02  |

```
strconv.FormatFloat(123.45, 'f', 2, 64) // "123.45"
strconv.FormatFloat(123.45, 'e', 2, 64) // "1.23e+02"
strconv.FormatFloat(123.45, 'g', -1, 64) // "123.45"
```

```
'f', 0 → "123"
'f', 2 → "123.45"
'f', 5 → "123.45000
```

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Understanding Data Type Conversion in Go

&nbsp;&nbsp;&nbsp;

**Topic Overview**

Greetings! Are you ready to delve deeper into the universe of the Go language? Our current journey delves into a fundamental concept of programming languages: Data Type Conversion. Often, we need to transform one data type into another, similar to adjusting a spaceship's asteroid-floating-point measurements to fit the integer-based radar system. We will study both automatic and explicit conversions, pointing out potential traps and loopholes along the way. Let's power up our knowledge engines!

**Automatic (Implicit) Conversions**

Unlike many other languages, `Go` does not provide automatic type conversions. Consequently, each conversion between different types requires explicit syntax. This might seem restrictive, but the approach is designed to prevent subtle bugs.

**Manual (Explicit) Conversions**

There will be instances when we will need to fit a large floating-point number into an integer-based tuple, requiring explicit casting. Observe how we convert a `float64` to an `int`:

```Go
var d float64 = 10.25  // a double number
var i int = int(d)  // casting the float64 to int

fmt.Println("The value of i:", i)  // Output: The value of i: 10
```

Notice that the fractional part `0.25` was discarded during the process, leaving only `10` as the result.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Converting to and from Strings: Integer**

A type of conversion that frequently occurs in Go is converting to and from string values. We often need to convert numbers to strings for output or parse strings as numbers for calculations. Note that you will need to import "strconv" to use functions for strings conversion. It will look like this:
```go
import (
    "fmt"
    "strconv"
)
```

Now, let's meet our string conversion functions in the following code snippet.
```go
var ten int = 10  // an integer with value 10
var tenString string = strconv.Itoa(ten)  // converting int to string
fmt.Println("The value of tenString:", tenString)  // Output: The value of tenString: 10

var twentyFiveString string = "25"
var twentyFive int
twentyFive, _ = strconv.Atoi(twentyFiveString)  // converting string to int
fmt.Println("The value of twentyFive:", twentyFive)  // Output: The value of twentyFive: 25

var invalidNumber string = "25abc"
var number int
number, _ = strconv.Atoi(invalidNumber)  // Oops! This will throw an error, "25abc" is not a number!
```
&nbsp;&nbsp;&nbsp;

For the conversion from string to `int`, we use `strconv.Itoa(i)`, and for the conversion from `string` to `int`, we use `strconv.Atoi(s)`. The last one returns two values: the converted string, and an error message if something goes wrong. That's why we assign it to two variables, `twentyFive, _`, where `twentyFive` will hold our value, and `_` will hold the error. Without error, `_` will simply contain `nil`. By naming error `_`, we highlight that we do not plan to use this error message, even if it is not empty.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Converting to and from Strings: Float**

You may want to convert strings to float64 numbers, or the other way around. Here is how to do it in Go:
```Go
var pi float64 = 3.14159 // a float value

// coverting float to string
var piString string = strconv.FormatFloat(pi, 'f', -1, 64)
fmt.Println("The value of piString:", piString)  // Output: The value of piString: 3.14159

var invalidFloatString string = "3.14abc"
var number float64
number, _ = strconv.ParseFloat(invalidFloatString, 64) // This will throw an error!
```
In `strconv.FormatFloat(f, 'f', -1, 64)`, `'f'` is the format, -1 allows any precision, and 64 specifies the bit size.

In `strconv.ParseFloat(s, 64)`, `64` is the bit size for the resultant floating-point number.

The `strconv` functions will return an error if the string is not a valid number. So, ensure your inputs are correctly formatted.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example:
```go
package main

import (
    "fmt"
    "strconv"
)

func main() {
    planetNumber := 7.0
    // TODO: Convert the planetNumber float64 to an int using explicit casting.
    var seven int = int(planetNumber)
    // TODO: Convert the int to a string using strconv.Itoa.
    var sevenString string = strconv.Itoa(seven)
    // TODO: Convert the string back to an int using strconv.Atoi.
    var planetCount int
    planetCount, _ = strconv.Atoi(sevenString)
    // Print out the int value after conversion.
    fmt.Println("In our galaxy, the planet count is:", planetCount)
}

```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example 2:
```go
package main

import (
    "fmt"
    "strconv"
)

func main() {
    var asteroidSize float64 = 9.75  // size of asteroid in kilometers
    // TODO: Convert the asteroidSize float value to a string without losing precision and store it in sizeString.
    var sizeString string = strconv.FormatFloat(asteroidSize, 'f', -1, 64)
    fmt.Println("Asteroid size in text is:", sizeString)  // Output should be: Asteroid size in text is: 9.75
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Navigating Go's Conditional Cosmos: Steering Through if-else and Beyond

&nbsp;&nbsp;&nbsp;

**Charting Our Coding Trajectory: Overview of Conditional Statements in Go**

Greetings, Go astronaut in training! Today's itinerary includes studying the mainstay of programming control flow: **conditional statements.** These mechanisms steer the course of our **Go** program. Are you strapped in and ready to explore the `if-else` statement? Let's start the countdown now!

&nbsp;&nbsp;&nbsp;

**Mapping the If and If-Else Constellation**

The structure of `if` and `if-else` control flow in Go reflects the following:
```Go
if condition {
    // action if condition is true
}

// additionally

if condition {
    // action if condition is true
} else {
    // action if condition is false
}
```
Here, when the given `condition` becomes true, we take action via the `if` block. When the `condition` is false, we have an optional `else` block to resort to.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Probing the Nebula of Go's If-Else Statement**

By using the `if` statement in Go, we command the machine to undertake specific actions only when conditions are met. Let's imagine deciding to land on a planet with breathable air:
```Go
var oxygenLevel = 78  // The level of oxygen on the planet

if oxygenLevel > 20 {
    fmt.Println("Planet has breathable air!") // Oxygen level is suitable
} else {
    fmt.Println("Oxygen level too low!") // Oxygen level is not high enough
}
// The code prints: Planet has breathable air!
```
In the example, the statement `if oxygenLevel > 20` tests if the oxygen level is greater than 20. If the test passes (`true`), it prints: `"Planet has breathable air!"`. If it fails (`false`), the `else` clause provides an alternative command and prints: `"Oxygen level too low!"`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Multiple Conditions: The Else If Statement**

When dealing with multiple conditions, we fall back on `else if`:
```Go
var oxygenLevel = 58
if oxygenLevel > 70 {
    fmt.Println("Excellent Oxygen level!")
} else if oxygenLevel > 50 {
    fmt.Println("Oxygen level is acceptable.")
} else {
    fmt.Println("Oxygen level is too low!")
}
// The code prints: Oxygen level is acceptable.
```
With the `else if` keyword, we can map out alternative routes until we find a fitting one, which allows us to adapt suitably to different levels of oxygen. As soon as one condition is met, the program disregards subsequent `else if` conditions.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Navigating Through Interstellar Switches**

In Go, a `switch` statement provides a way of checking multiple conditions in a concise and readable format. It's similar to `if`, `else if, else`, but more structured.

Here is the basic format of a `switch`:
```go
switch condition {
    case condition_1:
        // do something if condition_1
    case condition_2:
        // do something if condition_2
    default:
        // do something if none of the conditions are met
}
```
The `switch` tests a condition, and executes the block of code associated with the first `case` clause that is `true`. If no `case` condition is fulfilled, the `default` clause is executed.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Switch Example**

Let’s consider an example:
```go
var planet = "Mars"

switch planet {
    case "Earth":
        fmt.Println("Planet is Earth.")
    case "Mars":
        fmt.Println("Planet is Mars.")
    default:
        fmt.Println("Unidentified Planet.")
}
// The code prints: Planet is Mars.
```
Here, we examine the value of the `planet` variable. The `switch` tests each `case` invoking the first condition that matches the `planet` value. In this situation, it triggers `"Planet is Mars."`. If no match is found, the `default` clause executes, but in this case it is not required.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Multiple Values in Switch**

In Go, a `case` statement can have multiple comma-separated values. Let's consider an example:
```go
    food := "apple"

    switch food {
        case "apple", "banana", "orange":
            fmt.Println(food, "is a fruit.")
        case "carrot", "broccoli", "radish":
            fmt.Println(food, "is a vegetable.")
        case "chicken", "beef", "fish":
            fmt.Println(food, "is a meat.")
        default:
            fmt.Println("Unknown food category.")
    }

    // Output: apple is a fruit
```
If any of the values listed in the `case` statement matches our target variable, the code inside this `case` statement is executed.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example: Add Orbit Classification to the Cosmic Body Identifier
```go
package main

import "fmt"

func main() {
    cosmicBody := "Mars"
    
    // Checking the type of orbit using a switch case
    switch cosmicBody {
        case "Mercury", "Venus", "Earth", "Mars":
            // TODO: Print that this cosmic body orbits in the inner solar system.
            fmt.Println(cosmicBody, "orbits in the inner solar system.")
        // TODO: define the other case
        case "Jupiter", "Saturn", "Uranus", "Neptune":
            fmt.Println(cosmicBody, "orbits in the outer solar system.")
        default:
             // TODO: Handle the case where the cosmic body is unknown.
            fmt.Println("cosmic body is unknown.")
    }
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### String Manipulation in Go: Mastering Concatenation Operations

&nbsp;&nbsp;&nbsp;

**Understanding Concatenation**

Concatenation acts like glue, binding strings together to craft meaningful sentences. Suppose you have two strings — "Neil" and "Armstrong". We can link them to form a single string, "Neil Armstrong". Let's look at how:

```Go
firstName := "Neil"
lastName := "Armstrong"
fullName := firstName + " " + lastName   // Concatenation operation

fmt.Println(fullName)  // Output: Neil Armstrong
```

Here, the '+' operator attaches `firstName`, a space, and `lastName`, forming the `fullName` string. Simple, isn't it? You might have observed this technique in some of our earlier `fmt.Println` statements.

&nbsp;&nbsp;&nbsp;

**String Concatenation with '+' Operator in Go**

In Go, the '+' operator only allows the same data types specifically when concatenation is intended. Let's dig deeper with an example:
```Go
name := "Alice"
apples := 5
message := name + " has " + strconv.Itoa(apples) + " apples."  // Explicit conversion of 'int' to 'string'

fmt.Println(message)  // Output: Alice has 5 apples.
```
Pay close attention, as Go does not implicitly convert the integer `apples` to a string. We used `strconv.Itoa` for explicit conversion before performing the concatenation.

&nbsp;&nbsp;&nbsp;


**Journey with `strings.Builder` in Go**

In Go, the `string` object is immutable. You can't modify it directly once it's created. However, Go does provide efficient ways to modify strings. The `strings.Builder` is your friend when it comes to concatenating strings. It concatenates strings efficiently, without creating new objects with each operation.

First of all, let's import `"string"` to get the access to the `strings.Builder`:
```Go
import (
    "fmt"
    "strings"
)
```
Now, let's see it in action:
```Go
var sb strings.Builder
sb.WriteString("Hello, ")
sb.WriteString("World!")
sb.WriteString(" What ")
sb.WriteString("a wonderful ")
sb.WriteString("day out there!")
fmt.Println(sb.String())  // Output: Hello, World! What a wonderful day out there!
```
Do you see how we first created a `strings.Builder`, then used `WriteString` to add strings to it? The final combined string is produced with `sb.String()`.

`strings.Builder` provides a more efficient way to concatenate large amounts of strings than the '+' operator does. It's an excellent choice for efficient and versatile string manipulation!

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Leveraging `fmt.Printf` for Advanced String Formatting**

The `fmt.Printf` function is a powerful tool in Go's arsenal for dealing with strings, especially when it comes to sophisticated formatting. Unlike `fmt.Println`, which prints strings with a newline, `fmt.Printf` lets you format strings with placeholders and then inject variables into those placeholders, all in one swoop.

The `fmt.Printf` function offers a wide range of formatting verbs to handle different data types, such as integers, floating-point numbers, strings, and more. This flexible approach allows us to seamlessly integrate various data types into our strings without manual conversions.

When dealing with integers and doubles (floating-point numbers), `fmt.Printf` shines by allowing precise control over how these numbers are displayed within a string. Let's see some examples:
```Go
age := 3
fmt.Printf("Age: %d years old.\n", age)  // Age: 3 years old.

height := 30.32
fmt.Printf("Height: %.1f cm\n", height)  // Height: 30.3 cm

name := "Cosmo"
fmt.Printf("Name is %s\n", name)  // Name is Cosmo
```
Notice `\n` at the end of the string. It is a special symbol for the new line.

- `%d` is the verb used for integers. It formats and inserts an integer into the placeholder.

- `%f` is the verb for floating-point numbers. It can be further fine-tuned by specifying the precision (number of decimal places). For example, `%.1f` means one decimal place.

- `%s` is the verb for strings. It inserts a string into the placeholder.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


Example : String Building with Go's Builder
```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    var sb strings.Builder
    sb.WriteString("Hello, ")
    sb.WriteString("Explorer! ")
    sb.WriteString("Embrace the Go Challenge into the Future course.")
    fmt.Println(sb.String())
}
```

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example : Text Formatting with String Concatenation in Go
```go
package main

import (
    "fmt"
    "strconv"
    "strings"
)

func main() {
    userName := "Alex"
    unreadEmails := 23
    
    // TODO: Use the + operator to create a report with userName and unreadEmails.
    report1 := userName + " you have " + strconv.Itoa(unreadEmails) + " unread Mails."
    // Complete this with your concatenation code
    
    var excitement string = " Check your inbox now!"
    var builder strings.Builder
    
    // TODO: Use the strings.Builder to concatenate excitement message to the report.
    builder.WriteString(report1)
    builder.WriteString(excitement) // Replace this with the excitement string
    report2 := builder.String()
    
    fmt.Println(report2)
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example : Text Formatting with String Concatenation in Go
```Go
package main

import "fmt"

func main() {
    planet := "Earth"
    number := 7
    // TODO: Use fmt.Printf to state the planet's name and its number from the Sun using placeholders.
    fmt.Printf("state of planet %s and its %d from the Sun using placeholders.\n", planet, number)
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example : Add Score Display with Custom Message Using String Formatting in Go

```Go
package main

import (
    "fmt"
)

func main() {
    score := 93.2
    // TODO: Use fmt.Printf to format 'score' as a floating-point number with one decimal place and concatenate it with a custom message.
    //Output should be: Your score is 93.2%
    fmt.Printf("Your score is %.1f%%n", score)
}
```

`%.1f` → number with 1 decimal place

`%%` → outputs the % character

`\n` → line break

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


![img02](images/02.webp)

### Exploring Arrays in Go: Storage, Access, and Characteristics

**Introduction**

Welcome to our enlightening lesson about **z** in Go, a fundamental data structure. Think of arrays as a collection of lockers at a school, each storing a specific item. Arrays help us organize our data, facilitating easy retrieval and manipulation.

In this captivating journey, we'll unravel arrays in Go, focusing on their creation, element access, and unique properties.

**What are Arrays?**

An array is a container that holds a sequence of elements of the same type. Much like a bookshelf with specified slots, each housing a book, an array has numbered slots that store items (referred to as elements).

**Creating Arrays in Go**

Creating an array in Go is straightforward. The syntax is as follows:
```go
var arrayName [Size]Type
```
We can declare and initialize an array simultaneously like this:
```go
var hoursStudied = [7]int{2, 3, 4, 5, 6, 3, 4}
```
This statement declares the array **hoursStudied** and supplies it with the hours studied.

**Accessing Array Elements in Go**

Accessing the elements in an array is akin to fetching a specific book from a shelf. For example, to retrieve the first two days of study from our `hoursStudied` array, we would do the following:
```go
firstDayHours := hoursStudied[0]
fmt.Println("You studied", firstDayHours, "hours on the first day")


fmt.Println("You studied", hoursStudied[1], "hours on the second day")
```
Here, `[0]` fetches the first element and `[1]` fetches the second. Be careful not to access an index that is out of range; doing so would trigger a runtime error in Go.

**Exploring Array Properties in Go**
Arrays in Go have intriguing properties. Once an array is created, its size is fixed. This feature has certain implications when it comes to assigning one array to another. We use the len function to get the array's length:

```Go
fmt.Println("You studied for", len(hoursStudied), "days")
```
In the Go language, arrays are value types. Therefore, a copy of the original array is assigned when you assign one array to another.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### String Manipulation in Go: Mastering Concatenation Operations

**Understanding Concatenation**

Concatenation acts like glue, binding strings together to craft meaningful sentences. Suppose you have two strings — "Neil" and "Armstrong". We can link them to form a single string, "Neil Armstrong". Let's look at how:
```Go
firstName := "Neil"
lastName := "Armstrong"
fullName := firstName + " " + lastName   // Concatenation operation

fmt.Println(fullName)  // Output: Neil Armstrong
```
Here, the '+' operator attaches `firstName`, a space, and `lastName`, forming the `fullName` string. Simple, isn't it? You might have observed this technique in some of our earlier `fmt.Println` statements.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**String Concatenation with '+' Operator in Go**

In Go, the '+' operator only allows the same data types specifically when concatenation is intended. Let's dig deeper with an example:
```Go
name := "Alice"
apples := 5
message := name + " has " + strconv.Itoa(apples) + " apples."  // Explicit conversion of 'int' to 'string'

fmt.Println(message)  // Output: Alice has 5 apples.
```
Pay close attention, as Go does not implicitly convert the integer `apples` to a string. We used `strconv.Itoa` for explicit conversion before performing the concatenation.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Journey with `strings.Builder` in Go**

In Go, the `string` object is immutable. You can't modify it directly once it's created. However, Go does provide efficient ways to modify strings. The `strings.Builder` is your friend when it comes to concatenating strings. It concatenates strings efficiently, without creating new objects with each operation.

First of all, let's import `"string"` to get the access to the `strings.Builder`:

```Go
import (
    "fmt"
    "strings"
)
```
Now, let's see it in action:
```Go
var sb strings.Builder
sb.WriteString("Hello, ")
sb.WriteString("World!")
sb.WriteString(" What ")
sb.WriteString("a wonderful ")
sb.WriteString("day out there!")
fmt.Println(sb.String())  // Output: Hello, World! What a wonderful day out there!
```
Do you see how we first created a strings.Builder, then used WriteString to add strings to it? The final combined string is produced with sb.String().

strings.Builder provides a more efficient way to concatenate large amounts of strings than the '+' operator does. It's an excellent choice for efficient and versatile string manipulation!

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Leveraging `fmt.Printf` for Advanced String Formatting**

The `fmt.Printf` function is a powerful tool in Go's arsenal for dealing with strings, especially when it comes to sophisticated formatting. Unlike `fmt.Println`, which prints strings with a newline, `fmt.Printf` lets you format strings with placeholders and then inject variables into those placeholders, all in one swoop.

The `fmt.Printf` function offers a wide range of formatting verbs to handle different data types, such as integers, floating-point numbers, strings, and more. This flexible approach allows us to seamlessly integrate various data types into our strings without manual conversions.

When dealing with integers and doubles (floating-point numbers), `fmt.Printf` shines by allowing precise control over how these numbers are displayed within a string. Let's see some examples:
```Go
age := 3
fmt.Printf("Age: %d years old.\n", age)  // Age: 3 years old.

height := 30.32
fmt.Printf("Height: %.1f cm\n", height)  // Height: 30.3 cm

name := "Cosmo"
fmt.Printf("Name is %s\n", name)  // Name is Cosmo
```
Notice `\n` at the end of the string. It is a special symbol for the new line.

- `%d` is the verb used for integers. It formats and inserts an integer into the placeholder.

- `%f` is the verb for floating-point numbers. It can be further fine-tuned by specifying the precision (number of decimal places). For example, `%.1f` means one decimal place.

- `%s` is the verb for strings. It inserts a string into the placeholder.


### Understanding Multidimensional Arrays in Go

**Topic Overview and Introduction**

Hello and welcome! Today, we're diving into **multidimensional arrays** in Go, which are arrays of arrays. Their organizational structure is similar to a checkerboard, where each square can hold a value. By the end of our journey, you'll know how to create, initialize, and work with these arrays. Let's get started!

**Understanding Multidimensional Arrays**

A two-dimensional (2D) array in Go is much like a grid, consisting of multiple rows (the array) and columns (the arrays within the array).

Imagine a box of chocolates with several rows and columns - the box is the array, and each cell is an array element. In Go, a similar 2D array takes the form:
```go
var chocolates [3][3]string
```
`"chocolates"` here is a 2D array that can hold 9 strings in a 3x3 distribution.

**Creating and Initializing Multidimensional Arrays**

To fill our box of chocolates, we initialize our array:
```go
var chocolates = [3][3]string {
  {"dark", "white", "milk"},
  {"hazelnut", "almond", "peanut"},
  {"caramel", "mint", "coffee"}}
```
Each row (an array) holds several columns (array elements). Since Go arrays are of a fixed size, we define this size at the time of creation and cannot change it thereafter.

**Accessing Values in Multidimensional Arrays**

To access elements in a 2D array, two indices are used: the first is for the row and the second is for the column.
```go
selectedChoc := chocolates[0][2]
fmt.Println("Selected chocolate: ", selectedChoc)  // white
```

example:
```go
package main

import "fmt"

func main() {
    // TODO: Declare and initialize a two-dimensional array with your favorite sweets
    var sweets = [3][2]string { 
    // The array should have 3 rows and 2 columns
    {"dark", "white"},
    {"coffee", "milk"},
    {"caucus", "mindal"}}

    // TODO: Access and print a sweet of your choice from the array using its row and column indices
    fmt.Println("my choice from sweets", sweets[0][1])
}
```
About how the array is structured:
 
- The first index is the row.

- The second index is the column.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Creating and Manipulating Slices in Go: A Beginner's Guide

**Introduction**

Welcome to our session on **Go slices!** Slices, which are flexible data sequences in Go, are crucial for managing dynamic data sets. This lesson lays the groundwork for creating and manipulating slices in Go.

**Understanding Slices in Go**

Slices in Go are dynamic sequences of elements that provide the functionality and flexibility that arrays lack. While both slices and arrays allow for the collection of elements under a single name, their capabilities differ significantly. Arrays have a fixed size, defined at compile time, making them less optimal for sequences of elements whose size might change. Slices, on the other hand, are built on top of arrays to offer dynamic sizing. They can grow and shrink as needed, making them the go-to choice for dealing with sequences of data that require flexibility.

**Slices Properties**

A slice has three key properties: a pointer to an array (the underlying array where the elements of the slice are actually stored), a length, and a capacity. The `len()` function returns the number of elements in the slice, while the `cap()` function returns the maximum number of elements the slice can hold before needing to allocate a larger underlying array.
```go
var numbers []int // nil slice
fmt.Println(len(numbers))  // Output: 0  
fmt.Println(cap(numbers))  // Output: 0  
```

**Creating and Initializing Slices**

To create a slice, you can either define it directly with elements, use the `make` function, or slice an existing slice or array. The `make` function creates a slice with a predetermined length and capacity, making it immediately ready for use without specifying initial elements.
```go
numbers := make([]int, 5)  // A slice of five integers
fmt.Println(numbers)       // Output: [0 0 0 0 0]

numbers := []int{1, 2, 3, 4, 5}  // Directly initializing a slice with elements
fmt.Println(numbers)             // Output: [1 2 3 4 5]

subSlice := numbers[1:4]         // Creating a sub-slice, slicing from 1 to 3 index
fmt.Println(subSlice)            // Output: [2 3 4]
```

**Manipulating Slices: Adding Elements**

The `append()` function adds new elements to the end of a slice. If the underlying array of the slice has enough capacity to fit the new elements, `append()` will use it; otherwise, it will allocate a new array, copy the existing elements, and add the new ones. This automatic handling of array resizing is what makes slices so versatile.
```go
numbers = append(numbers, 6, 7) // Appending new elements
fmt.Println(numbers)            // Output: [1 2 3 4 5 6 7]
```
**Manipulating Slices: Removing Elements**

Although Go does not have a built-in function to remove elements from a slice directly, you can achieve this by utilizing slicing and the `append()` function. To remove elements, you create a new slice that skips over the elements you intend to remove.
```go
// Removing the element at index 2
numbers = append(numbers[:2], numbers[3:]...)  // Creates a new slice by appending elements after index 2 to those before index 2
fmt.Println(numbers)                           // Output: [1 2 4 5 6 7]
```
This method splits the slice around the element(s) to be removed and then combines the two parts. This operation does not immediately free the memory of the removed element(s) as they might still be part of the underlying array, highlighting the importance of understanding slice internals to manage memory effectively.

`...` in the line` numbers = append(numbers[:2]`, `numbers[3:]...)` is used to add elements from a slice or array to another slice one by one. The `...`' effectively tells the append function to unpack the elements of `numbers[3:]` (which is a slice) and treat them as separate arguments. Without `...`, the append function would treat `numbers[3:]` as a single slice argument.

**Copying Slices and Capacity**

You might sometimes need to copy one slice to another when working with slices. The `copy()` function facilitates this, allowing you to duplicate the elements of a source slice to a destination slice. The number of elements copied is the minimum of `len(src)` and `len(dst)`.
```go
moreNumbers := make([]int, len(numbers))  // Creating a new slice
copy(moreNumbers, numbers)                // Copying elements from numbers to moreNumbers
fmt.Println(moreNumbers)                  // Output: [1 2 4 5 6 7]
```


example1:

```go
package main

import "fmt"

func main() {
    // Imagine managing a restaurant menu: we start with some dishes and append new ones
    menu := []string{"Pizza", "Burger", "Salad"}
    fmt.Println(menu) // Output: [Pizza Burger Salad]

    // Adding a new dish to the menu
    menu = append(menu, "Pasta")
    fmt.Println(menu) // Output: [Pizza Burger Salad Pasta]

    // Removing the 'Burger' from the menu
    menu = append(menu[:1], menu[2:]...)
    fmt.Println(menu) // Output: [Pizza Salad Pasta]
}
```

example2 :
```go
package main

import (
    "fmt"
)

func main() {
    // Restaurant Menu Management
    menu := []string{"Pizza", "Pasta", "Salad", "Burger"}   // Initial menu
    fmt.Println("Original Menu:", menu)
    
    // Removing the 'Salad' from the menu
    indexToRemove := 2
    lastItemIndex := 2
    menu = append(menu[:indexToRemove], menu[indexToRemove+1:]...) // Bug introduced here

    
    fmt.Println("Updated Menu:", menu)
    fmt.Println("Last menu item:", menu[lastItemIndex]) // Attempt to access the last menu item
}
```




### Overview and Introduction to Maps in Go

Welcome aboard for our journey with Go's **maps**. Maps, a built-in data type, make it easy to organize and find data within collections — a crucial feature for effective programming.

In Go, maps link one value (the key) to another value. Just imagine having a roster of students alongside their grades. With maps, you can quickly associate each student (the key) with their corresponding grade (the value).

Our quest today involves the following:

- Understanding how to craft and initiate Go's maps.

- Learning to interact with map elements.

- Fathoming the external behavior of maps as reference types in Go.

Let's dive in!

**Declaring and Initializing Maps in Go**

Declaring a map in Go is as easy as pie. For this purpose, you can utilize either the make function or a composite literal. Let's sketch an illustration:
```go
// Using the make function
var grades1 = make(map[string]int)

// Using composite literal syntax
var grades2 = map[string]int{}

// Pre-populated map using composite literal syntax
var grades3 = map[string]int{"John": 85, "Jane": 90}
```
As displayed above, we create maps that accommodate strings as keys and integers as their associated values.

**Working with Maps**

Go's maps enable swift and effortless handling of map elements. Adding, changing, accessing values, or handling absent keys in a map is a cinch:

```Go
var grades = make(map[string]int)
grades["John"] = 85   // Store John's grade in the map
grades["Jane"] = 90   // Store Jane's grade

johnGrade := grades["John"]     // Retrieve John's grade
grades["John"] = 95             // Update John's grade

delete(grades, "John")            // Delete John's entry
johnGrade, johnExists := grades["John"]   // Check if John's grade exists
if (johnExists) {
    fmt.Println(johnGrade)  // won't execute, as John was deleted
}
```
Notice how we use the key to access, update or delete an element. It is much more handy then indicies in the array. We can access `"John"`'s grade using his own name as an identifier!

**Maps and Reference Types**

Since maps are reference types in Go, assigning a map to a new map variable creates a reference, not a replica:
```go
var originalData = map[string]int{"apple": 1, "banana": 2}
var copiedData = originalData
copiedData["apple"] = 100

fmt.Println(originalData)
fmt.Println(copiedData)
```

The output of both maps reflects the value 100 related to "apple". Here, `copiedData` is simply a reference to `originalData`, hence modifications enacted on `copiedData` affect `originalData`.




Example 1:
```go
package main

import "fmt"

func main() {
  // Classroom management with map structure
  classroomGrades := map[string]int{"Alice": 92, "Bob": 88, "Eve": 79}

  // Updating Eve's grade to an 82
  classroomGrades["Eve"] = 82
  
  // Printing the updated classroom grades
  fmt.Println(classroomGrades)
}
```
Example 2:
```go
package main

import "fmt"

func main() {
    // Let's simulate a simple classroom grading system.
    classroomGrades := make(map[string]int)
    classroomGrades["Alice"] = 92
    classroomGrades["Bob"] = 85
    classroomGrades["Charlie"] = 78
    
    // TODO: Update Bob's grade to a better one.
    classroomGrades["Bob"] = 90
    // TODO: Print Charlie's grade and Bob's grade to the console.
    fmt.Println("Bob:", classroomGrades["Bob"], "Charlie:", classroomGrades["Charlie"])
}

```





### Mastering the Versatility of Go's `for` Loop

**Introduction**

Greetings, Future Coder! Today, we're going to deepen our knowledge of the **Go programming language** as we explore loop structures. Think of loops as roads you walk down, going in circles until you find your way out - or in our case, satisfy a condition. The Go language uses only one looping keyword, the for loop, and we're about to see how it efficiently covers the functionalities typically provided by while and do-while loops in other languages.


**Introduction to `for` Loops Handling `while` Functionality**

In Go programming, a `for` loop can handle tasks usually performed by a `while` loop in many other languages. This is done by using only the conditional part of the `for` loop.

Here is the `for` loop's structure, which mimics a while loop:
```Go
for condition {
    do some action
}
```

Below is a simple `for` structure that acts like a while loop, counting down from 5 to 0:
```Go
countdown := 5
for countdown >= 0 {
    // The countdown will print the current number and decrease it by 1 in each loop
    fmt.Println(countdown)
    countdown--
}
// Output:
// 5
// 4
// 3
// 2
// 1
// 0
```
Take note of the decrementing command `countdown--`. Without it, our code would become an infinite loop, so be careful!

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**When to Use the `while` Loop (Mimicking with `for` in Go)**

Unlimited Iteration: `While` loops are handy in scenarios where the number of iterations is unknown beforehand. In Go, we would implement this with `for`:
```Go
for condition {
    // process
}
```
1.Continuous Checking: Sometimes, we may need to keep checking a particular state or condition repeatedly (without having a counter), and as soon as it changes, we stop. This usage would also use the `while` like structure in Go.

```Go
for conditionStillTrue() {
    // process
}
```
In conclusion, although Go does not include an explicit `while` loop, its versatile `for` loop covers the need for it seamlessly. We can mimic the `while` loop's functionalities when necessary through condition only for' loops.



example1:
```go
package main

import "fmt"

func main() {
    // Create a slice representing planets in a distant galaxy
    planets := []string{"Zebes", "Hoth", "Coruscant", "Gallifrey", "Vulcan", "Cybertron", "Krypton", "Pandora"}
    
    // Using basic for-loop to iterate through all planets
    for i := 0; i < len(planets); i++ {
        fmt.Println("Visiting planet: " + planets[i])
    }
    fmt.Println()
    
    // Use range clause to visit each planet and print its name
    for _, planet := range planets {
        fmt.Println("Visiting planet: " + planet)
    }
}
```

example 2:
```go
package main

import "fmt"

func main() {
    planets := []string{"Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune"}
    for i := 0; i < len(planets); i++ {
    
    fmt.Printf("%d -  Exploring planet:%s\n", i+1, planets[i])
    }
}
```


example 3:
```Go
package main

import "fmt"

func main() {
    // TODO: Create a slice with the names of the planets of our solar system
    // Here is the list: Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
    planets := []string{"Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune"}
    // TODO: Use a for loop to visit (print the name of) each planet, **in reversed order**
    for i := len(planets) - 1; i >= 0; i-- {
        fmt.Println("Visiting planet: " + planets[i])
    }
    // Neptune should go first, then Uranus, etc.
    // Hint: for decrementing the variable in the loop, use `i--`, which is the same as `i = i - 1`
}
```


example 4:
```go
package main

import "fmt"

func main() {
    // TODO: Create a variable to keep track of the number of stars collected.
  stars := 0
    // TODO: Write a for loop that simulates the collection of stars until you have 5. 
    for stars <=5 {
        fmt.Println(stars)
        stars++
    }
        // In each iteration, print the current number of stars collected and then increment the count.

}
```

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Mastering Conditional Logic in Loops with Go


**Reviewing Go Loop Structure**

Before we forge ahead, it's essential to revisit the foundation: loops in Go. Go provides a highly versatile `for` loop. It's used not only for iterating over arrays, slices, and maps but also for emulating a `while` loop.

A `for` loop in Go iterates a predetermined number of times, much like a reliable spaceship following a set route:
```Go
for i := 0; i < 5; i++ { // Iterates five times
    fmt.Println(i) // Prints 0 to 4
}
```

To act like a while loop, our for loop eliminates the initialization and increment portions:
```Go
i := 0
for i < 5 { // Condition for the loop to continue
    fmt.Println(i) // Prints 0 to 4
    i++ // Increment the counter
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Reviewing Go Conditional Statements**

Now, let's revisit the `if-else` construct, which is Go's means for making decisions.
```Go
asteroidsDistance := 10

if asteroidsDistance > 15 {
    fmt.Println("Navigate through the asteroids.")
} else {
    fmt.Println("Steer clear of the asteroids.")
}
```
The `if-else` statement enables the spaceship to decide whether to navigate through the asteroids based on their distance.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Combining Loops and Conditional Statements**

Next, let's consider how the for loop integrates with an if-else statement:
```Go
for i := 0; i < 6; i++ {
    if i % 2 == 0 {
        fmt.Println(i, "is even.")  // prints for 0, 2, and 4
    } else {
        fmt.Println(i, "is odd.")  // prints for 1, 3, and 5
    }
}   
```

Similarly, we can use an `if-else` statement within a for loop, which emulates a `while` loop:
```Go
i := 0
for i < 7 { 
    if i % 3 == 0 {
        fmt.Println(i, "is divisible by 3.")  // will print for 0, 3, 6 
    } else {
        fmt.Println(i, "is not divisible by 3.")  // will print for 1, 2, 4, 5
    }
    i++
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Real-life Examples: Part 1**

Consider an application designed to monitor oxygen levels across a network of stations. These stations are critical for ensuring the safety and well-being of personnel in a space habitat. The following example demonstrates how we can iterate through an array of oxygen sensor readings and apply conditional logic to alert us to potential dangers.
```Go
oxygenReadings := []float64{21.5, 20.9, 19.2, 18.0, 22.1} // Oxygen levels of different stations
safeOxygenLevel := 19.5 // Minimum safe level in percentage

for i, reading := range oxygenReadings {
    if reading < safeOxygenLevel {
        fmt.Printf("Warning! Low oxygen level at Station %d. \n", i+1)
        // will print for stations 3 and 4
    } else {
        fmt.Printf("Station %d oxygen level is safe.\n", i+1)
        // will print for stations 1, 2 and 5
    }
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Real-life Examples: Part 2**

In another scenario, consider a game. As long as the game is on—represented by a `for` loop—if you hit an alien, represented as an `if` condition, you gain points!

For random generation of successful/unsuccessful hit, we will need to import `"math/rand"`:
```Go
import (
    "fmt"
    "math/rand"   
)
```
```Go
score := 0
gameOn := true

for gameOn { 
    isAlienHit := rand.Intn(2) // Random generator for hit (1) or miss (0)

    if isAlienHit == 1 {
        fmt.Println("Alien vessel hit! +10 points")
        score += 10
    } else {
        fmt.Println("Missed! Game Over.")
        gameOn = false
    }
}
fmt.Println("Your score is", score) // Displays the final score when the game ends.
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example :
```Go
package main

import (
    "fmt"
)

func main() {
    for star := 1; star <= 5; star++ {
        if star % 2 == 0 {
            fmt.Println("Star", star, "is a Binary Star!")
        } else {
            fmt.Println("Star", star, "is a Pulsar.")
        }
    }
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;
























example1:
```go
package main

import "fmt"

func main() {
    temperatures := []int{21, 19, 20, 22, 18, 17, 19}
    for _, temp := range temperatures {
        // TODO: Check if the temperature is between 18 and 22 degrees inclusive and print the appropriate message.
        if temp <= 22 && temp >= 18{
            fmt.Println(temp, "degrees - Keep your helmet on.")
        }
        
    }
}
```

example 2 :
```go
package main

import "fmt"

func main() {
    temperatures := []int{21, 19, 20, 22, 18, 17, 19}
    for _, temp := range temperatures {
        // TODO: Check if the temperature is between 18 and 22 degrees inclusive and print the appropriate message.
        if temp >=18 && temp <= 22 {
             fmt.Println(temp, "the temperature is between 18 and 22 degrees.")
        } else {
             fmt.Println(temp, "degrees - Keep your helmet on.")
        }
        
    }
}
```


Example:
```Go
package main

import (
	"fmt"
)

func main() {
    // TODO: Set a slice of fuel levels for each day of the week (7 elements)
     fuelLevels := []int{85, 70, 55, 74, 68, 88, 100}
    // TODO: Use a for loop to go through each day of the week
    for index, fuel := range fuelLevels {
         // TODO: Use an if-else statement to check if the fuel levels are enough for the mission
         // Let's say that the acceptable minimum fuel level is 80        
        if fuel < 80 {
         // TODO: Print the day and whether the fuel levels are not enough or satisfactory
            fmt.Printf("Day %d: %d - Not enough fuel\n", index+1, fuel)
        } else {
            fmt.Printf("Day %d: %d - Fuel level is satisfactory\n", index+1, fuel)
        }
    }
}
```  


```Go
package main

import (
    "fmt"
)

func main() {
    meals := []string{"Breakfast", "Lunch", "Dinner"}
    tasks := []string{"Plan", "Cook"}

    for _, meal := range meals {
        // TODO: Loop through the tasks and output "<Task> <Meal>", e.g. "Plan Breakfast"
        for _, task := range tasks {
            fmt.Printf(" : %s %s \n", task, meal)
        }
    }
}
```














&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Mastering Nested Loops in Go


**Nested Loops: The Basics**

Suppose your day involves multiple tasks like cooking and eating for each meal: breakfast, lunch, and dinner. In this case, the meals represent the outer loop, while the tasks constitute the inner loop. Similarly, in Go, we can write a `for` loop (inner loop) inside another `for` loop (outer loop).

```Go
for initialization; condition; iteration {
    // outer loop code
    for initialization; condition; iteration {
        // inner loop code
    }
}
```
The program evaluates the condition of the outer loop. If it's true, it enters the loop and executes the **inner** loop to completion before moving on to the next iteration of the **outer** loop.

**Go Nested `for` Loops**

Writing nested **for** loops in Go is straightforward. To demonstrate, let's print a 5x5 star pattern using nested loops:

```Go
for i := 0; i < 5; i++ {
    for j := 0; j <= i; j++ {
        fmt.Print("* ") // print "* "
    }
    fmt.Println() // move to the next line
}
// Prints:
// *
// * *
// * * *
// * * * *
// * * * * * 
```
In this instance, the outer loop governs the rows, while the inner loop controls the columns. The result is a diagonal pattern of stars printed in the console!

**Emulating `while` Loops in Go**

As Go doesn't feature a distinct `while` keyword, we utilize the `for` loop to mimic the behavior of a `while` loop. Nested `for` loops that emulate `while` loops function precisely like the nested `for` loops we covered earlier.
```Go
i := 5
for i > 0 {
    j := i
    for j > 0 {
        fmt.Print(j, " ") // print the number
        j--
    }
    fmt.Println() // move to the next line
    i--
}
// Prints:
// 5 4 3 2 1
// 4 3 2 1
// 3 2 1
// 2 1 
// 1 
```
Upon executing this, you'll notice five lines, each containing decreasing numbers, just as the comment explains.


**Advanced Tasks with Nested Loops**

Nested loops are particularly effective for tasks such as traversing multi-dimensional arrays and executing complex searches.

Given a 2D slice, let's print all elements using nested loops:
```Go
intArray := [][]int{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}

for i := 0; i < len(intArray); i++ { // iterates over rows
    for j := 0; j < len(intArray[i]); j++ { // iterates over columns
        fmt.Print(intArray[i][j], " ") // prints each element
    }
    fmt.Println() // moves to the next line
}
// Prints:
// 1 2 3
// 4 5 6
// 7 8 9
```
To search for an integer in a 2D slice, nested loops again come in handy. Here's a demonstration that searches for the number `7`:
```Go
intArray := [][]int{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}
searchNumber := 7
isFound := false

for i := 0; i < len(intArray); i++ { // iterates over rows
    for j := 0; j < len(intArray[i]); j++ { // iterates over columns
        if intArray[i][j] == searchNumber {
            fmt.Println("Number", searchNumber, "found at [", i, ", ", j, "]")
            isFound = true
        }
    }
}

if !isFound {
    fmt.Println("Number", searchNumber, "not found in the array.")
}
// Prints: Number 7 found at [ 2 , 0 ]
```
Upon running the code, our nested loops locate and identify the number `7` in the third row.

**Quick Nested Loops Tips and Warnings**

Even though nested loops are incredibly useful, be cautious to avoid pitfalls such as infinite loops. A loop will turn into an infinite one if it isn't designed carefully to eventually terminate. Remember, proper controls and conditional statements are crucial.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example 1:
```Go
package main

import (
    "fmt"
)

func main() {
    // Daily meal preparation routine for three days
    days := []string{"Monday", "Tuesday", "Wednesday"}
    meals := []string{"Breakfast", "Lunch", "Dinner"}

    for i := 0; i < len(days); i++ {
        fmt.Println("Day:", days[i])
        for j := 0; j < len(meals); j++ {
            fmt.Println(" - Preparing", meals[j])
        }
    }
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example 2:
```Go
package main

import "fmt"

func main() {
    for i := 5; i > 0; i-- {
        fmt.Println("Countdown", i, ":")
        for j := i; j > 0; j-- {
            fmt.Println("- ", j)
        }
    }
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example 3:
```Go

package main

import "fmt"

func main() {
    i := 5
    for i > 0 {
        fmt.Println("Countdown", i, ":")
        j:= i
        for j > 0 {
            fmt.Println("- ", j)
            j--
        }
        i--
    }
}

```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example 4:
```Go
package main

import "fmt"

func main() {
    daysOfWeek := []string{"Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"}
    mealsOfTheDay := []string{"Breakfast", "Lunch", "Dinner"}

    // Outer loop for days of the week
    for i := 0; i < len(daysOfWeek); i++ {
        // Inner loop for each meal of the day
        for j := 0; j < len(mealsOfTheDay); j++ {
            fmt.Println("Day:", daysOfWeek[i] + ", Meal:", mealsOfTheDay[j])
        }
    }
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Mastering Loop Control with Break and Continue in Go

**Break Statement**

You can liken the break command to the moment when the music stops in a game of musical chairs, prompting you to leave the loop. It ends the loop, irrespective of the original condition of the loop.

Here is a quick example:

```Go
for i := 0; i < 10; i++ {
    if i == 7 { // When `i` reaches 7
        fmt.Println("7 found! Break!") // Message before the break
        break                           // Terminating the loop
    }
    fmt.Println("Number:", i) // Print i until we hit "break"
}
// Prints:
// Number: 0
// Number: 1
// Number: 2
// Number: 3
// Number: 4
// Number: 5
// Number: 6
// 7 found! Break!
```
Our loop operates on numbers from 0 through 6 and breaks when it reaches 7, thereby exiting early and skipping all remaining iterations.

**Continue Statement**
The `continue` keyword in Go can be compared to bypassing a boring view during a walk. It disregards the current loop iteration and moves ahead to the next one.

Here is an example:
```Go
for j := 1; j <= 10; j++ {
    if j == 4 || j == 7 { // Skip the 4th and 7th buildings
        continue
    }
    fmt.Println("Admiring building number:", j) // Continue with the rest
}
// Prints:
// Admiring building number: 1
// Admiring building number: 2
// Admiring building number: 3
// Admiring building number: 5
// Admiring building number: 6
// Admiring building number: 8
// Admiring building number: 9
// Admiring building number: 10
```
Our output confirms that we admire all buildings except numbers 4 and 7, which our `continue` statement skips.


**Break and Continue in Nested Loops**

Nested loops are loops within loops. In these loops, `break` and `continue` work in distinct ways. It's important to understand that both `break` and `continue` will exit or skip only their respective inner loop, not affecting the outer loop. Let's illustrate this with a couple of examples.

Consider a nested loop running on a `5x5` grid.

```Go
for i := 1; i <= 5; i++ {
    fmt.Print(i, ": ")
    for j := 1; j <= 5; j++ {
        if i == 3 && j == 3 {
            // break the inner loop
            break
        }
        fmt.Print(j, " ")
    }
    fmt.Println()
}
// Prints:
// 1: 1 2 3 4 5
// 2: 1 2 3 4 5
// 3: 1 2
// 4: 1 2 3 4 5
// 5: 1 2 3 4 5
```
In this context, break ends the inner loop when i and j both equal 3. Thus, when i becomes 3, the inner loop runs only up to j = 2 and then terminates. However, the outer loop continues until i = 5.

Meanwhile, let's introduce 'continue' in a similar setup.
```Go
for i := 1; i <= 5; i++ {
    fmt.Print(i, ": ")
    for j := 1; j <= 5; j++ {
        if i == 3 && j == 3 {
            continue
        }
        fmt.Print(j, " ")
    }
    fmt.Println()
}
// Prints:
// 1: 1 2 3 4 5
// 2: 1 2 3 4 5
// 3: 1 2 4 5
// 4: 1 2 3 4 5
// 5: 1 2 3 4 5
```
When continue encounters the i = 3, j = 3 condition, it skips the rest of the code inside its loop and swiftly moves to the next iteration. In this case, it means we omit printing j when both i and j are equal to 3.






Example 1:
```Go
package main

import (
    "fmt"
)

func main() {
    // Digital Excursion Loop Controller
    var totalLoops = 10
    var loop = 1
    for loop <= totalLoops {
        // TODO: Bypass the glitchy loop #4
        if loop == 4 {
            loop++
            continue
        }
        fmt.Printf("Navigating loop number: %d\n", loop)
        // TODO: Initiate cool-down procedure and terminate loop after loop #7
        if loop == 7 {
            fmt.Printf(" loop stoping %d\n", loop)
            break
        }
        loop++
    }
}
```

example 2: 
```Go
package main

// TODO: Implement your main function below
import "fmt"


func main() {
    // TODO: Create a loop for the amusement park rides from 1 to 10
    for i := 1; i <= 10; i++ {
    // TODO: Skip ride #6, it's under maintenance right now
        if i == 6 {
            continue
        }
        // TODO: End your day early when reaching ride #9
        if i == 9 {
            break
        }
        // TODO: Print out the ride numbers that visitors enjoy on the way
        fmt.Println("out the ride numbers that visitors enjoy on the way", i)
        continue
        
    }
   
}
```






















Example err5:
```go
package main

import (
    "fmt"
)

func main() {
    earthMass := 5.97e24
    celestialBodyMass := map[string]float64{"Mars": 6.39e23, "Jupiter": 1.898e27}

    // TODO: Calculate the earth-to-mars mass ratio and handle a potential missing key error for Mars
    marsMass, ok := celestialBodyMass["Mars"]
    if ok {
        fmt.Printf("The mass ratio of Earth to Mars is: %g\n", earthMass/marsMass)
    } else {
        fmt.Println("Error: The mass of Mars is not available in the map.")
    }
}
```
























Mastering String Operations in Go
Lesson Introduction
Welcome! Today, we're going to delve into the crucial operations associated with Strings in Go. Strings are fundamental in most programming languages as they are used for displaying and manipulating textual data. In this lesson, you'll learn about the basic string operations in Go, such as concatenation, comparison, and the use of common functions from the strings package.

Revising String Concatenation
Even though we already know what concatenation is and how it works, revisiting it strengthens our understanding! Concatenation — the process of joining items together — is a principal string operation. In Go, we achieve string concatenation by using the + operator.

```Go
package main

import "fmt"

func main() {
    var hello = "Hello, "
    var world = "World!"
    var greeting = hello + world

    fmt.Println(greeting) // "Hello, World!"
}
```
In this case, "Hello, " and "World!" were combined to form the string "Hello, World!".

Comparing Strings
There are often times when we need to compare strings. Fortunately, in Go, the simple comparison operators == and < work perfectly fine.

```Go
package main

import "fmt"

func main() {
    var firstWord = "Hello"
    var secondWord = "Hello"
    var areEqual = firstWord == secondWord

    fmt.Println(areEqual) // Outputs: true
}
```
Here, as firstWord and secondWord are equal, areEqual is true.

The < operator is used to determine if one string is alphabetically before the other.
```Go
package main

import "fmt"

func main() {
    var firstWord = "Apple"
    var secondWord = "Banana"
    var isLess = firstWord < secondWord
    
    fmt.Println(firstWord, "is less than", secondWord, "?", isLess) // Outputs: Apple is less than Banana? true
}
```
As you can see, the comparison result is true, which means that alphabetically, "Apple" comes before "Banana", because A comes before B. A string that would come before another string in the dictionary is considered less than the other. In case of a tie, Go will compare the following letter. For example, "Apple" will be less than "Application", because e is less than i. As the first four letters in the words are equal, Go compares the fifth one.

Important String Functions: len
In Go, unlike some other languages, strings do not have built-in methods. However, the Go Standard Library provides a package named strings which contains many useful string-related functions. Among them, some functions are pretty common:

len(): This function returns the number of characters in a string.
```go
package main

import "fmt"

func main() {
    var word = "Hello"
    var length = len(word)

    fmt.Println(length) // 5
}
```
mportant String Functions: ToLower and ToUpper
strings.ToLower() and strings.ToUpper(): These functions return the string either in lowercase or in uppercase, respectively.

```Go
package main

import (
    "fmt"
    "strings"
)

func main() {
    var word = "Hello"
    var lowerCaseWord = strings.ToLower(word)
    var upperCaseWord = strings.ToUpper(word)

    fmt.Println(lowerCaseWord) // "hello"
    fmt.Println(upperCaseWord) // "HELLO"
}
```

Important String Functions: TrimSpace
strings.TrimSpace(): This function removes all white spaces at the beginning and the end of a string.
```Go
package main

import (
    "fmt"
    "strings"
)

func main() {
    var sentence = " Hello, World!   "
    var trimmedSentence = strings.TrimSpace(sentence)

    fmt.Println(trimmedSentence) // "Hello, World!"
}
```







Example: Mastering String Operations in Go 2
```
package main

import "fmt"

func main() {
    firstWord := "Cosmic Go"
    secondWord := "Astronomy for Dummies"
    areEqual := firstWord == secondWord
    isLess := firstWord < secondWord

    // Output the comparison result
    // TODO: Using the IF-ELSE statement, change the output to print explanation like "'<Title 1>' is equal to '<Title 2>'." or "'<Title 1>' comes before/after '<Title 2>' alphabetically." instead
    if areEqual {
        fmt.Printf("'%s' is equal to '%s'.\n", firstWord, secondWord)
    } else if isLess {
        fmt.Printf("'%s' comes before '%s' alphabetically.\n", firstWord, secondWord)
    } else {
        fmt.Printf("'%s' comes after '%s' alphabetically.\n", firstWord, secondWord)
    }
}
```

Example: Mastering String Operations in Go 2
```Go
package main

import (
    "fmt"
    "strings"
)

func main() {
    // A book title in mixed case
    var bookTitle = "Golang For Beginners"
    var catalogTitle = strings.ToUpper(bookTitle) // TODO: Use the appropriate function to convert `bookTitle` to uppercase
    var searchKey =  strings.ToLower(bookTitle) // TODO: Use the appropriate function to convert `bookTitle` to lowercase

    fmt.Println(catalogTitle)
    fmt.Println(searchKey)
}
```



















```Go
package main

import "fmt"

func main() {
    // Below string represents a simple formatted text editing
    // that might be seen in a document editor.
    // It uses the newline and tab special character sequences.
    fmt.Println("Title:\tGo String Manipulation\n\nContent:\n\tGo strings are powerful.\n\tThey can contain \"special characters\" like newline (\\n) and tab (\\t).")
}
```

Example : Special Character Sequences in Go
```Go
package main

import "fmt"

func main() {
    // TODO: Display the document title, followed by sections with appropriate tabulations and a conclusion, all separated by newlines.
    // Here is an example of the output:
    // Title: Go String Manipulations
    //     - Introduction
    //     - Special Characters
    //     - Practice Exercises
    // Conclusion: Mastery of Go strings!
    fmt.Println("Title: Go String Manipulations\n \t- Introduction\n \t- Special Characters\n \t- Practice Exercises\nConclusion: Mastery of Go strings!")
}
```

Example : Mastering String Search and Replace in Go
```Go
package main

import (
    "fmt"
    "strings"
)

func main() {
    // A message in an online chat room:
    message := "Go is great, but please refrain from using bad words."

    // Moderating the chat by finding and replacing inappropriate language:
    moderatedMessage := strings.ReplaceAll(message, "bad words", "****")
    // Do one more change to replace "refrain from" with "avoid"
    moderatedMessage = strings.ReplaceAll(moderatedMessage, "refrain from", "avoid")
    // Display the moderated message:
    fmt.Println(moderatedMessage)
}
```

Example: Mastering String Search and Replace in Go
```Go
package main

import (
    "fmt"
    "strings"
)

func main() {
    chatMessage := "Go is cool! But sometimes, go can be challenging."

    // TODO: Add a condition to check if 'chatMessage' contains the word "go" (case insensitive)
    if strings.Contains(strings.ToLower(chatMessage), "go") {
        chatMessage = strings.ReplaceAll(chatMessage, "go", "Go")
    }
        // TODO: If it does contain, add a line to replace all occurrences of this word "go" with "Go"
    
    fmt.Println(chatMessage) // Should replace and output: "Go is cool! But sometimes, Go can be challenging."
}
```
```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    message := "The quick brown fox jumps over the lazy dog."
    forbiddenWord := "lazy"
    // TODO: Check if the message contains the forbidden word.
    if strings.Contains(message, forbiddenWord) {
        message = strings.ReplaceAll(message, forbiddenWord, "****")
    }
    // TODO: If it does, replace the forbidden word with "****".
    // TODO: Print the censored message.
    fmt.Println(message)
}
```
Example: Mastering String Search and Replace in Go

```Go
package main

import (
    "fmt"
    "strings"
)

func main() {
    chatMessage := "That was epic! epic win! so epic!"
    // TODO: Update the chat message by replacing the first instance of "epic" with "amazing"
    chatMessage = strings.Replace(chatMessage, "epic", "amazing", 1)
    fmt.Println(chatMessage) // This line remains unchanged. Update the variable 'chatMessage' above.
}
```


Example: Splitting and Joining Strings in Go

```Go
package main

import (
    "fmt"
    "strings"
)

func main() {
    quote := "To be, or not to be: that is the question."
    
    // TODO: Split the quotation into a slice of words
    slice := strings.Split(quote, " ")

    // TODO: Use strings.Join() to concatenate the last three words into a short snippet
    last := strings.Join(slice[7:10], " ")    
    // TODO: Now, print out the snippet of the joined three last words
    fmt.Println(last)
}
```

Example: Splitting and Joining Strings in Go
```Go
package main

import (
	"fmt"
	"strings"

	
)

func main() {
    bookQuote := "To be or not to be that is the question"

    // TODO: Use the strings.Split function to divide the quote into individual words and store them in a slice.
    words := strings.Split(bookQuote, " ")
    // TODO: Replace the last word ("question") with the word "boolean"
    words[len(words)-1] = "boolean"
    // TODO: Use the strings.Join function to recreate the quote from the slice of words.
    formattedQuote := strings.Join(words, " ")
    fmt.Println(formattedQuote) // Should print "To be or not to be that is the boolean"
}
```




Example : Exploring Functions in Go

```Go
package main

import (
    "fmt"
)

// TODO: Define a function that calculates the total price of an order
func calculateTotal(cosmicBurger float64, nebulaFries float64, starlightDrink float64) float64 {
   // This function should take three arguments: the price of a cosmic burger, nebula fries, and a starlight drink
  // TODO: Specify the prices for a cosmic burger, nebula fries, and a starlight drink
    return cosmicBurger + nebulaFries + starlightDrink   
}


// TODO: Call your function with the prices of the items and assign the result to a variable
func main() {
    // TODO: Print out the total cost of the order
    total := calculateTotal(10.00, 5.00, 3.00)
    fmt.Println(total)
}
```


Example : Calling Functions and Managing Multiple Return Values in Go
```Go
package main

import "fmt"

func checkOxygenLevel(level int) string {
    if level > 95 {
        return "Oxygen level is good for launch."
    }
    return "Oxygen level is too low for launch!"
}

func prepareForLaunch() {
    fmt.Println("Pre-launch check:")
    oxygenStatus := checkOxygenLevel(98)  // We use a constant value for the example
    fmt.Println(oxygenStatus)
}

func main() {
    prepareForLaunch()
}
```

Example: Calling Functions and Managing Multiple Return Values in Go
```Go 
package main

import "fmt"

func checkSuitabilityForLaunch(temperature int) bool {
    return temperature >= 15 && temperature <= 35
}

func launchReadinessCheck() {
    temperature := 20 // This is a constant value for the temperature
    // TODO: Implement the function call that checks if the temperature is suitable for launch
    if checkSuitabilityForLaunch(temperature) {
        fmt.Println("Launch is allowed ")
    } else {
        fmt.Println("Launch is not allowed")
    }
}
func main() {
    launchReadinessCheck()
}
```

Example : Navigating Go's Built-in Functions and Packages
```Go
package main

import (
    "fmt"
    "sort"
)

func main() {
    planetDistances := []float64{0.39, 0.72, 1.0, 1.52, 5.2, 9.58, 19.2, 30.05} // distances from the Sun in astronomical units
    sort.Float64s(planetDistances) // Sorting in ascending order
    fmt.Println(planetDistances) // Output: [0.39, 0.72, 1.0, 1.52, 5.2, 9.58, 19.2, 30.05]
}
```


Example : Navigating Go's Built-in Functions and Packages
```Go
package main

import (
    "fmt"
    "math"
    "sort"
)

func main() {
    planets := []string{"Mercury", "Venus", "Earth", "Mars"}
    orbitDistances := []float64{57.9, 108.2, 149.6, 227.9} // distances in million kilometers

    fmt.Println(len(planets))
    // TODO: Sort the orbitDistances in ascending order
    sort.Float64s(orbitDistances)
    // TODO: Round the first element in orbitDistances to the nearest whole number and print it
    fmt.Println(math.Round(orbitDistances[0]))
    // TODO: Print the sorted orbit distances
    fmt.Println(orbitDistances)
}
```



example : 
```
package main

import "fmt"

func travelToGalaxy(galaxy string, commanders ...string) {
    var commander string
    if len(commanders) > 0 {
        commander = commanders[0]
    } else {
        commander = "Commander Go"
    }
    
    fmt.Printf("%s will lead the expedition to %s.\n", commander, galaxy)
}

func main() {
    travelToGalaxy("Andromeda", "voyage commander")
}
```
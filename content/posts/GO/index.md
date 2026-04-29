---
title: "GO"
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

# Introduction to Programming with Go

![img01](images/01.webp)

## Taking Your First Step in Go Programming

Can you feel the excitement coursing through your veins? You're on the verge of diving into the world of programming. Let's get started!

#### What You'll Learn

**Go** is a powerful language that enables us to build a range of applications and solve real-world problems. As we embark on this journey, we'll first learn to create a basic program in Go — a simple `fmt.Println` statement. This program will offer a friendly greeting to the world, setting the pace for our future explorations.


Here's a sneak peek:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Learner!")
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Understanding the Code**

The code snippet contains some elements that we will discuss briefly:

- **package main:** Since your program is meant to run, it must be named `main`. We are focusing on running our program so for now just know that this is the name that must be used in order to run the program.

- **import "fmt":** We are printing something as output on the screen, and the built-in package `fmt` gives us access to this functionality.

- **func main():** This defines the `main` function, which is the entry point of every executable Go program.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### A Glimpse into Variables in Go

#### A Glimpse into Variables
Are you as excited as I am to advance to the next part of our learning journey? The topic of this lesson is **variables** — a core concept that you will use constantly in your programming career.

#### Meet the Variables
In programming, a **variable** is akin to a container for storing information. Various types of information — such as words, numbers, and more — can be stored in this container. Here is an example of how to create a variable in Go:

```go
package main

import "fmt"

func main() {
    var destination string
    destination = "Paris"
    fmt.Println(destination)
}
// Output: Paris
```

We've created a variable named `destination`. This process is known as **declaring a variable**. When declaring, we choose a fitting name for the container of information, followed by the type of data we expect to store. In Go, we declare a variable using the var keyword, followed by the variable name and finally its type. Following the declaration, we stored the string `"Paris"` in it. We **assign a value** to a variable with the assignment operator `=`. The variable that should store the value is on the left, while to acutal value is to the right.

This way of declaring a variable first and then assigning the value can get cumbersome after a while. Alternatively, we can use the shorthand operator `:=` to both declare and initialize the variable, which allows Go to **infer the type**:

```go
package main

import "fmt"

func main() {
    destination := "Paris"
    fmt.Println(destination)
}
// Output: Paris
```

Using this syntax, we forgo both the `var` keyword as well as the type declaration. The Go compiler is smart enough to know that if you are assigning a value within double quotes to a variable, the type should be a `string`. In Go, the term string refers to a sequence of characters. Any text no matter the lenght is considered a string as long as it is surrounded by double quotes.

Be careful! This shorthand only works within functions (in our example, we are writing code in the `main` function). If you are declaring variables out of a function, you need to use the full syntax, with the caveat that the value can be assigned at declaration time:

```Go
package main

import "fmt"

var destination string = "Paris"

func main() {
    fmt.Println(destination)
}
// Output: Paris
```

For now we will only use variables within functions, but it is good to keep this detail in mind if you get stuck declaring variables outside of functions.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

#### The Impact of Variables

Variables play a fundamental role in most programming languages, including Go. They help reduce complexity and make our programs more readable and maintainable. For instance, if we aim to fly to various locations instead of just Paris, we can simply change the value of the variable `destination` without having to modify our entire code.

In Go, variables always have a type, meaning that once your variable is declared, no other type of data can be assigned to it. This feature ensures type safety and can help prevent errors.

So, why are variables significant? They are among the fundamental building blocks of coding. Mastering them will put you on the right path to becoming a proficient programmer. Are you excited? Let's delve into the practice session and explore the world of Go variables together!

&nbsp;&nbsp;&nbsp;

EXAMPLE: 
```go
package main

import "fmt"

func main() {
    // Declare a variable and assign it the city "New York"
    var myFavoriteDestination string = "New York"

    // Display the value of the variable
    fmt.Println(myFavoriteDestination)
}

```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Managing Quantities with Variables in Go


Are you eager to expand your knowledge of Go programming? Fantastic! In this lesson, we'll focus on using `variables` in Go to manage quantities. This is a crucial feature for organizing data and performing computations. Go emphasizes type safety, ensuring that the data you work with is consistent and reliable.

**Getting Into The Nitty-Gritty**

To manage quantities efficiently, we use `variables`. In Go, a variable is a name that represents or references a value stored in the system memory. A variable can represent numbers, strings, and more. You can use the named variable to read the value stored, or to modify it as needed.

For instance, let's consider an example where we are planning a world trip and want to track the number of days we intend to spend in each country. We could use `variables` in Go to do this effectively:

```go
package main

import "fmt"

func main() {
    // Initializing variables for each country
    daysInFrance := 7
    daysInAustralia := 10
    daysInJapan := 5

    // Summing up the total
    totalDays := daysInFrance + daysInAustralia + daysInJapan

    // Using fmt.Println to display the result
    fmt.Println("The total number of days for the trip will be:", totalDays)
}
```

This code will produce the following output when executed:

```
The total number of days for the trip will be: 22
```
&nbsp;&nbsp;&nbsp;

Note that in the `fmt.Println` function, we have included multiple arguments, which are individual pieces of data. These arguments are separated by commas. This demonstrates Go's capability to concatenate, or merge, arguments in the `fmt.Println` function. You can change the order of arguments to alter the output sequence.

Notice also that `fmt.Println` automatically adds a space between the arguments, ensuring the output looks tidy and pleasant.

An argument can be a direct value, like a number or a string, as well as a variable containing some data.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Combining Text with Variables in Go Using fmt.Sprintf

**What You'll Learn**

Are you ready to explore combining text with variables in Go using `fmt.Sprintf`? This versatile feature makes it effortless to create dynamic and readable strings by embedding variables directly within them.

**Code Example**

For a trip planning scenario, consider these variables:

```go
// Travel variables
var destination string = "Paris"
var numberOfDays int = 5
var tripCost float64 = 1200.50 // Notice how we can store numbers in variables that are not integers!
```

Using `fmt.Sprintf` enables you to neatly include variable values in your text output:


```go
// Using fmt.Sprintf for travel details
package main

import (
    "fmt"
)

func main() {
    var destination string = "Paris"
    var numberOfDays int = 5
    var tripCost float64 = 1200.50

    details := fmt.Sprintf("I am planning a trip to %s.", destination)
    fmt.Println(details)
    details = fmt.Sprintf("The trip will be for %d days.", numberOfDays)
    fmt.Println(details)
    details = fmt.Sprintf("The estimated cost of the trip is $%.2f.", tripCost)
    fmt.Println(details)
}
```
output:

```
I am planning a trip to Paris.
The trip will be for 5 days.
The estimated cost of the trip is $1200.50.
```
Format specifiers in `fmt.Sprintf` help format the inserted variables in specific ways:

- `%s`: Used for strings. In our code, %s is replacedwith the value of destination.

- `%d`: Used for integers. In our code, %d inserts the value of numberOfDays.

- `%f`: For floating-point numbers. By default, it includes six decimal places.

- `%.2f`: This limits the floating-point number to two decimal places.In our code, `tripCost` is limited to only 2 decimal places.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

EXAMPLE:
```go
package main

import (
    "fmt"
)

func main() {
    // TODO: Modify the values to reflect the new travel plans
    var destination string = "Tokyo"
    var numberOfDays int = 6
    var tripCost float64 = 1800.50

    // Printing the travel details using fmt.Sprintf
    details := fmt.Sprintf("I am planning a trip to %s.", destination)
    fmt.Println(details)
    details = fmt.Sprintf("The trip will be for %d days.", numberOfDays)
    fmt.Println(details)
    details = fmt.Sprintf("The estimated cost of the trip is $%.2f.", tripCost)
    fmt.Println(details)
}

```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

EXAMPLE 2:
```go
package main

import (
    "fmt"
)

func main() {
    // Variables related to travel
    var destination string = "Tokyo"
    var numberOfDays int = 7
    var tripCost float64 = 1500

    // TODO: Update the trip cost to cover a friend as well
    tripCost = tripCost * 2

    // Printing the travel details using fmt.Sprintf
    fmt.Println(fmt.Sprintf("I am planning a trip to %s with a friend.", destination))
    fmt.Println(fmt.Sprintf("The trip will last for %d days.", numberOfDays))
    fmt.Println(fmt.Sprintf("The estimated cost of the trip is $%.2f.", tripCost))
}

```

In Go, arithmetic operations allow for direct manipulation of numeric values within variables, using familiar symbols: `+` for addition, `-` for subtraction, `*` for multiplication, and `/` for division. Here are concise examples for each:


- Addition (`+`): `myVariable = myVariable + 10` increases `myVariable` by 10.

- Subtraction (`-`): `myVariable = myVariable - 5` decreases `myVariable` by 5.

- Multiplication (`*`): `myVariable = myVariable * 2` doubles `myVariable`.

- Division (`/`): `myVariable = myVariable / 2` halves `myVariable`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Introduction to Go Variables and Booleans

An exciting lesson awaits us, promising a deeper exploration of Go's **variables** and **Boolean types**. In earlier lessons, we covered the basics of Go, and now we're going to build on that by exploring how to use **Boolean variables**. These are simple yet powerful, used to represent a condition or status as either `true` or `false`.

**What You'll Learn**

In this lesson, we'll define two string variables: each will hold the name of a destination. We'll also explore how to use **Boolean variables** — a type of variable that can only be `true` or `false`, similar to a light switch that can only be either on (`true`) or off (`false`). Here's what it looks like in Go:

```go
package main

import (
    "fmt"
)

func main() {
    destinationA := "Paris"
    destinationB := "Tokyo"
    
    hasVisitedA := true
    hasVisitedB := false

    fmt.Println(destinationA, "visited:", hasVisitedA)
    fmt.Println(destinationB, "visited:", hasVisitedB)
}
```
Here, `destinationA` and `destinationB` store strings representing travel destinations. On the other hand, `hasVisitedA` and `hasVisitedB` are Boolean variables that, like an on-off switch, inform us whether these destinations have been visited.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

#### Getting Boolean Values from Expressions

In Go, Boolean values can be derived from various expressions using comparison operators. These operators compare values and return a Boolean result (`true` or `false`). Common comparison operators include:

- `==` for equality

- `!=` for inequality

- `<` for less than

- `<=` for less than or equal to

- `>` for greater than

- `>=` for greater than or equal to

Here's how you can use these operators in Go:

```go
package main

import (
    "fmt"
)

func main() {
    // Comparing numbers
    a := 5
    b := 10
    
    isEqual := a == b
    isNotEqual := a != b
    isLessThan := a < b
    isGreaterThan := a > b
    
    fmt.Println("Is equal:", isEqual)              // false
    fmt.Println("Is not equal:", isNotEqual)       // true
    fmt.Println("Is less than:", isLessThan)       // true
    fmt.Println("Is greater than:", isGreaterThan) // false

    // Comparing strings
    name1 := "Alice"
    name2 := "Bob"

    areNamesEqual := name1 == name2
    
    fmt.Println("Are names equal:", areNamesEqual) // false
}
```

These expressions are essential for making decisions in programming, enabling you to direct the flow of your program based on conditions.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

# Introduction to Simple Data Structures in Go

![img02](images/02.webp)

## Introduction to Slices in Go

&nbsp;&nbsp;&nbsp;

**Introduction to Creating and Accessing Slices**

Welcome to an exciting journey in the world of programming with Go.

&nbsp;&nbsp;&nbsp;

Today, we will explore one of Go's key features: data structures, specifically **slices**. **Slices** are an integral part of Go, providing the functionality you need to manage collections of data efficiently. Let's dive into the world of **slices**!

&nbsp;&nbsp;&nbsp;

**Packing Our Travel Bags with Slices!**

In Go, a **slice** is a flexible and powerful data structure that allows you to work with sequences of data. It's similar to creating a travel list of all the locations you'll visit in your adventure. For this travel-themed exploration, we'll create a **slice** of travel destinations. Go's strong typing ensures that all elements in a **slice** are of the same type.

Consider the following **slice** of travel destinations to explore:
```go
package main

import (
    "fmt"
)

func main() {
    travelDestinations := []string{"Paris", "Sydney", "Tokyo", "New York", "London"}

    fmt.Println("Our travel destinations are:", travelDestinations)
}
```
This **slice** contains five elements, all of which are strings. Note the syntax here: elements in a **slice** are enclosed in curly braces `({})`, and we use the `[]` notation to declare a **slice**.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

#### Accessing the Elements of Travel

Accessing elements from a **slice** is like selecting a travel destination from your list. Go uses a zero-index system, meaning the first element within a **slice** is accessible via the index `0`. Let's see how you can access different elements from our **slice**.

To access an element from the **slice**, you refer to its index. For instance, if you want the first destination on our list, which is "Paris," you can do the following:

```go 
package main

import (
    "fmt"
)

func main() {
    travelDestinations := []string{"Paris", "Sydney", "Tokyo", "New York", "London"}

    firstDestination := travelDestinations[0]
    fmt.Println("The first destination on the list is:", firstDestination)
}
```
Note that Go does not support negative indexing to access elements from the end of a **slice**. To access the last element, you'll directly use its position:
```go
package main

import (
    "fmt"
)

func main() {
    travelDestinations := []string{"Paris", "Sydney", "Tokyo", "New York", "London"}

    lastDestination := travelDestinations[len(travelDestinations)-1]
    fmt.Println("The last destination on the list is:", lastDestination)
}
```
In this code, `len(travelDestinations)` returns the length of the **slice**, and we adjust the index with `-1` to get the last element.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

EXAMPLE1 :
```go
package main

import (
    "fmt"
)

func main() {
    // Our travel destinations
    travelDestinations := []string{"Paris", "Sydney", "Tokyo", "New York", "London"}

    // Finding the third destination in the slice
    thirdDestination := travelDestinations[2]
    fmt.Println("The third destination on the list is:", thirdDestination)
}
```

EXAMPLE2 
```go
package main

import (
    "fmt"
)

func main() {
    // TODO: Establish a slice to store travel destinations
    // Hint: Use the '[]string{}' syntax to create a slice of strings
    travelDestinations := []string{"Oslo", "Copenhagen", "Reykjavik", "Helsinki", "Stockholm"}
    // TODO: Find the first destination in the slice and print it
    // Hint: In Go, slices are zer-indexed
    firstDestination := travelDestinations[0]
    lastDestination := travelDestinations[len(travelDestinations) -1]
    // Hint: Use len(slice_name) to determine the last index and access the element
    fmt.Println("The first and last destination on the list is:", firstDestination, lastDestination)
    
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Adding and Removing Items from Slices in Go

Welcome back, traveler! As part of our travel-themed journey, we'll be managing a slice of countries for our hypothetical world tour! Just as it is in real-world travel, our itinerary may change, prompting us to add or remove countries from our slice. In Go, slices provide a powerful way to work with collections of elements. Today, we'll explore how to append new items and manually remove items from slices.

&nbsp;&nbsp;&nbsp;

 **What You'll Learn**

Let's learn how to manipulate slices in Go, focusing on how to add and remove items. We will use the built-in function `append()`, which is used to add an item to the end of a slice. For removing items, we'll demonstrate slicing techniques to manually handle the removal of elements.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

#### Slicing with Indexes in Go

Before we cover modifying slices, let's talk about slicing slices! By using slicing syntax, you can create a new slice based on the elements between specified indexes of an existing slice. Here's how slice indexing works:

1. **Basic Slicing Syntax**:

- The slicing operation is performed using a colon : inside square brackets to specify the start and end indexes. The syntax follows the format `slice[startIndex:endIndex]`.

```go
numbers := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}

subSlice := numbers[2:5] // Creates a new slice with elements {2, 3, 4}
```

- **Start Index**: Specifies the index at which the new slice begins (inclusive).

- **End Index**: Specifies the index at which the new slice ends (exclusive). The element at `endIndex` is not included in the new slice.


2. **Omitting Indexes**:

- If the start index is omitted, it defaults to `0`, i.e., the beginning of the original slice.

- If the end index is omitted, it defaults to the length of the original slice, i.e., the end of the original slice.

```go
fromStart := numbers[:4]  // Creates a new slice with elements {0, 1, 2, 3}
toEnd := numbers[6:]      // Creates a new slice with elements {6, 7, 8, 9}
entireSlice := numbers[:] // Creates a copy of the entire slice {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
```
Slicing with indexes is an essential feature in Go that provides flexibility in accessing and manipulating collections of data efficiently. By understanding how to define start and end points, you can leverage slices to perform complex data operations seamlessly.

&nbsp;&nbsp;&nbsp;

**Using the "append" Function**

Now, let's take a closer look at how to use `append()` in various ways while working with slices:

```go
package main

import "fmt"

func main() {
    numbers := []int{1, 2, 3}

    // Append a single element
    numbers = append(numbers, 4)

    // Append multiple elements
    numbers = append(numbers, 5, 6, 7)

    // Append another slice
    moreNumbers := []int{8, 9, 10}
    numbers = append(numbers, moreNumbers...)

    // Remove element '5'
    indexToRemove := 4 // index of the element '5'
    numbers = append(numbers[:indexToRemove], numbers[indexToRemove+1:]...)
}
```
1. **Initial Slice**:

- `numbers := []int{1, 2, 3}` initializes a slice of integers with the elements `1`, `2`, and `3`.

2. **Appending a Single Element**:

- `numbers = append(numbers, 4)` appends the integer `4` to the end of the `numbers` slice. Notice that we assign the result of the `append` function to the `numbers` variable. This is because `append` doesn't modify the original slice, but rather returns the new structure as its result. We then choose to store it in the same variable so that it holds the updated state of the slice.

3. **Appending Multiple Elements**:

- `numbers = append(numbers, 5, 6, 7)` appends three elements: `5`, `6`, and `7` to the end of the slice. The append function can efficiently handle multiple elements in a single call.

4. **Appending Another Slice**:

- `moreNumbers := []int{8, 9, 10}` creates another slice named moreNumbers.

- `numbers = append(numbers, moreNumbers...)` appends all elements of the `moreNumbers` slice to numbers. The `...` operator is used to unpack `moreNumbers` and append all of its elements as individual arguments to `append`. It is a convenient way of supplying all of the elements of a slice one by one to the append function.

5. **Removing an Element**:

- To remove the element 5, we first identify its index, `indexToRemove := 4`.

- `numbers = append(numbers[:indexToRemove], numbers[indexToRemove+1:]...)` slices the existing `numbers` slice into two parts: up to but not including index `4`, and from index `5` to the end. The two slices are then appended together, effectively removing the element at index `4`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

#### Slice Operations in Our World Tour Slice

Now that we have learned all of the cool things we can do with slices and the append function, consider our world tour slice:

```go
package main

import "fmt"

func main() {
    // Initial slice of countries for the world tour.
    worldTourCountries := []string{"Italy", "France", "USA", "Brazil", "India", "China"}

    // Append "Australia" to the end of the slice.
    worldTourCountries = append(worldTourCountries, "Australia")

    // Append "Spain" to the end of the slice, following "Australia".
    worldTourCountries = append(worldTourCountries, "Spain")

    // To remove "Brazil", create a new slice excluding that element.
    worldTourCountries = append(worldTourCountries[:3], worldTourCountries[4:]...)

    // To remove "China", create a new slice excluding that element.
    worldTourCountries = append(worldTourCountries[:4], worldTourCountries[5:]...)

    // Insert "Japan" at the beginning of the slice.
    worldTourCountries = append([]string{"Japan"}, worldTourCountries...)

    fmt.Println(worldTourCountries)
}
```

Let's walk through the code to explain what it does:

1. **Initial Slice of Countries**:  Our first line initializes a slice of strings containing the countries planned for the world tour.

2. **Appending "Australia"**: We use the `append()` function to append "Australia" to the end of the `worldTourCountries` slice. The `append` function returns a new slice with the added element.

3. **Appending "Spain"**: Similarly, we append "Spain" to the end of the slice.

4. **Removing "Brazil"**: To remove "Brazil", we use slicing. This line splits the slice into two parts, before and after "Brazil", using indexes before `3` and after `4`. The `...` is used to unpack the second part so it can be appended to the first.

5. **Removing "China"**: Similarly, we remove "China" by splitting the slice at indexes before `4` and after `5`, appending them together.

6. **Inserting "Japan" at the Beginning**: Next, we prepend "Japan" to the slice. We create a new slice containing "Japan" and append the entire existing `worldTourCountries` slice by unpacking it with `...`.

7. **Printing the Final Slice**: Finally, we output the modified slice to the console, showing the updated list of countries in the world tour.


By practicing different scenarios, you will strengthen your understanding of these operations, which are vital for slice manipulation in Go.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

EXAMPLE 1: 
```go
package main

import "fmt"

func main() {
    // Current plan for the world tour
    worldTourCountries := []string{"Italy", "France", "USA", "Brazil", "India", "China"}

    // TODO: Visit Japan before Italy
    worldTourCountries = append([]string{"Japan"}, worldTourCountries...)
    // TODO: Visit Spain after all other countries
    worldTourCountries = append(worldTourCountries, "Spain")

    fmt.Println(worldTourCountries)
}
```
&nbsp;&nbsp;&nbsp;

EXAMPLE 2:
```go
package main

import "fmt"

func main() {
    // Current plan for the world tour
    worldTourCountries := []string{"Italy", "France", "USA", "Brazil", "India", "China", "Australia", "Spain"}

    // TODO: we want to add Japan to the beginning of our world tour
    worldTourCountries = append([]string{"Japan"}, worldTourCountries...)
    
    // Printing the updated list of countries
    fmt.Println(worldTourCountries)
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

EXAMPLE 3: 
```go
package main

import "fmt"

func main() {
    // Current plan for the world tour
    worldTourCountries := []string{"Italy", "France", "USA", "Brazil", "India", "China", "Australia", "Spain"}

    // TODO: Remove Australia from the tour.
    indexToRemove := 6
    worldTourCountries = append(worldTourCountries[:indexToRemove], worldTourCountries[indexToRemove+1:]...)
    // Print the updated tour plan.
    fmt.Println(worldTourCountries)
}
```

EXAMPLE 4: 
```go
package main

import "fmt"

func main() {
    // Initial slice of countries planned for the world tour
    worldTourCountries := []string{"Italy", "France", "USA", "Brazil", "India", "China"}

    // TODO: Add "Australia" and "Spain" at the end of the slice
    worldTourCountries = append(worldTourCountries, "Australia", "Spain")
    
    // TODO: Remove "Brazil" and "China" from the slice
    worldTourCountries = append(worldTourCountries[:3], worldTourCountries[4:]...)
    worldTourCountries = append(worldTourCountries[:4], worldTourCountries[5:]...)
    // TODO: Add "Japan" at the beginning of the slice
    worldTourCountries = append([]string{"Japan"}, worldTourCountries...)
    
    // Print the final world tour plan
    fmt.Println(worldTourCountries)
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Exploring Arrays in Go

Welcome to our continuing adventure through Go's data structures! In this lesson, we'll shift our focus from slices to **arrays**. Let's dive in!

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Creating an Array**

**Arrays** in Go are a fundamental way to store multiple items in a single variable, but unlike slices, their size is fixed upon creation. Arrays are explicitly defined with square brackets that specify the number of elements they hold. Let's track the cities you've planned to visit on your travels using an array:
```go
package main

import "fmt"

func main() {
    // Create an array of cities
    citiesPlanned := [5]string{"Paris", "Tokyo", "New York", "London", "Berlin"}

    // Access and print an element from the array
    fmt.Println("The second city planned to visit is:", citiesPlanned[1])
}
```
&nbsp;&nbsp;&nbsp;

#### Accessing Array Elements

Now, we'll demonstrate how to access individual cities within your `citiesPlanned` array. Each element is accessed using its index, similar to how you'd access elements in slices:
```go
// Access and print specific elements
fmt.Println("First city:", citiesPlanned[0])
fmt.Println("Third city:", citiesPlanned[2])
fmt.Println("Fifth city:", citiesPlanned[4])
```
This approach allows you to directly access specific elements when you know their index positions.

&nbsp;&nbsp;&nbsp;

#### Array Size Immutability

It's crucial to understand that while the contents of an array can be altered, its size is **immutable** and cannot change after initialization. This means you need to know the number of items in advance:
```go
// Change an element in the array
citiesPlanned[2] = "Sydney"
fmt.Println("Updated city:", citiesPlanned[2])
```
**Arrays** offer a reliable way to store a predetermined number of items and provide benefits in terms of memory allocation efficiency for fixed-size collections.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

EXAMPLE:
```go
package main

import "fmt"

func main() {
    // TODO: Create an array of cities
    newCitiesVisited := [5]string{"Bangkok", "Dubai", "Sydney", "Toronto", "Cape Town"}

    // TODO: Access elements in the array and print them
    fmt.Println("First city visited:", newCitiesVisited[0])
    fmt.Println("Last city visited:", newCitiesVisited[4])
}
```

&nbsp;&nbsp;&nbsp;

EXAMPLE 2: 
```go
package main

import "fmt"

func main() {
    // My array of cities I have visited
    citiesVisited := [5]string{"Paris", "Tokyo", "New York", "London", "Berlin"}

    // Print the first city visited
    fmt.Println("First city visited:", citiesVisited[0])

    // Access the sixth city...


    // Print the last city visited
    fmt.Println("Last city visited:", citiesVisited[4])
}
```
&nbsp;&nbsp;&nbsp;

EXAMPLE 3 :
```go
package main

import "fmt"

func main() {
    // TODO: Create an array of continents to visit
    continents := [6]string{"Africa", "Asia", "Antarctica", "Europe", "North Amerioca", "South America"}

    // TODO: Print the first and last continents from the array
    fmt.Println("The first city planned to visit is:", continents[0])
    fmt.Println("The last city planned to visit is:", continents[5])

}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Exploring Maps in Go

&nbsp;&nbsp;&nbsp;

**Welcoming Maps**

Having explored slices and arrays, let's take another step in our Go journey. Imagine you're traveling and need to remember the capitals of various countries. While you could memorize each one individually, wouldn't it be more efficient to have a **map** that links each country to its capital? In Go, **maps** allow you to establish such relationships between keys and values, like countries and their capitals.

Here's how we create a **map** in Go:
```go
// Creating a simple map to hold the country as key and its capital as value
capitalCities := map[string]string{
    "France": "Paris",
    "Japan": "Tokyo",
    "Kenya": "Nairobi",
}
```

In Go, declaring a map involves specifying the types for both the keys and the values that the map will hold. The `map` keyword is followed by square brackets containing the key's type, while the value's type follows outside the brackets. In our sample code, both the key and the value have the type `string`.

In the upcoming practice tasks, we will guide you through the creation, access, and manipulation of elements within `maps`, ensuring you are equipped to harness the capabilities of Go.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

#### Adding a New Value to a Map

To add a new value to a **map**, you simply assign a value to a new key. If the key doesn't already exist in the map, it will be added.

```go
// Initializing the map
capitalCities := map[string]string{
    "France": "Paris",
    "Japan": "Tokyo",
    "Kenya": "Nairobi",
}

// Adding a new key-value pair
capitalCities["Germany"] = "Berlin"
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

#### Accessing a Specific Value with a Key

You can access a specific value in a **map** using its key. If the key exists, you'll get the corresponding value.

```go
// Accessing the value associated with the key "Japan"
capital := capitalCities["Japan"]
fmt.Println("The capital of Japan is:", capital)  // Output: The capital of Japan is: Tokyo
```

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

#### Updating an Existing Value

To update an existing value in a **map**, use the key to assign a new value.

```Go
// Updating the capital of France
capitalCities["France"] = "Marseille"

// Verify the update
fmt.Println("The updated capital of France is:", capitalCities["France"])  // Output: The updated capital of France is: Marseille
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

#### Removing a Value

To remove a key-value pair from a map, use the delete function, specifying the **map** and the key.

```go
// Removing the entry for "Kenya"
delete(capitalCities, "Kenya")

// Verify the removal
capital, exists := capitalCities["Kenya"]
if !exists {
    fmt.Println("The capital of Kenya does not exist in the map.")
} else {
    fmt.Println("The capital of Kenya is:", capital)
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

EXAMPLE :
```go
package main

import "fmt"

func main() {
    // Creating a map to hold the country as key and its capital as value
    capitalCities := map[string]string{
        "France": "Paris",
        "Japan": "Tokyo",
        "Kenya": "Nairobi",
    }

    // TODO: Add the USA with its capital to the map
    capitalCities["USA"] = "Washington D.C."

        
    // TODO: Access and print the capital city of USA
    fmt.Println("The updated capital of France is:", capitalCities["USA"])
    
}
```
&nbsp;&nbsp;&nbsp;

EXAMPLE 2:
```go
package main

import (
    "fmt"
)

func main() {
    // Creating a map to hold country as key and its capital as value
    travelMap := map[string]string{
        "France": "Paris",
        "Japan": "Tokyo",
        "USA": "Washington D.C.",
        "Australia": "Canberra",
    }

    // TODO: Remove Japan and its capital from the map
    delete(travelMap, "Japan")

    
    // TODO: Displaying the map
    fmt.Println("Displaying the map", travelMap)
}
```
&nbsp;&nbsp;&nbsp;

EXAMPLE 3:
```go
package main

import (
    "fmt"
)

func main() {
    // A map of countries we've visited and their capitals
    visitedCapitals := map[string]string{
        "Spain":    "Madrid",
        "Thailand": "Bangkok",
        "Egypt":    "Cairo",
        "Australia": "Sydney", // This is an incorrect entry
    }

    // TODO: Correct the capital of Australia in the map
    visitedCapitals["Australia"] = "Canberra"
    // Displaying the updated map
    fmt.Println("Countries and their capitals:", visitedCapitals)
}
```
&nbsp;&nbsp;&nbsp;

EXAMPLE 4:
```go
package main

import "fmt"

func main() {
    // TODO: Create a map of countries and their capital cities
    CapitalCity := map[string]string{
        "France": "Paris",
        "Japan": "Tokyo",
        "Kenya": "Nairobi",
    }
    // TODO: Print out the capital of France
    fmt.Println("Capital city of France:", CapitalCity["France"])
    // TODO: Add the USA
    CapitalCity["USA"] = "Washington D.C."
    // TODO: Remove Kenya
    delete(CapitalCity, "Kenya")

    // TODO: Display the updated map
    fmt.Println("Display the updated map:", CapitalCity)

}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Exploring Nested Maps in Go: Creating Complex Data Structures

Welcome back! You’ve started with **maps** in Go; now, let's delve deeper. In this unit, we’ll explore **nested maps** — think of them as a multi-level travel guide with detailed information on various destinations around the globe.

&nbsp;&nbsp;&nbsp;

**What You'll Learn**

Moving beyond the basic key-value pairs, we'll focus on **nested maps** in Go. Imagine organizing airports worldwide, each described by its code, location, and available amenities, all within nested structures. We'll cover:

- Creating **nested maps** for structured, multi-level data organization in Go.

- Accessing, manipulating, and updating data within these complex **map** structures.

- Efficiently adding new information to **nested maps**.

&nbsp;&nbsp;&nbsp;

Here's a simple example to showcase how to model **nested maps** in Go:
```go
package main

import "fmt"

func main() {
    // Creating a nested map to represent airports
    airports := map[string]map[string]string{
        "JFK": {
            "Location":  "New York, USA",
            "Amenities": "Free WiFi, Lounges",
        },
        "LHR": {
            "Location":  "London, UK",
            "Amenities": "Shopping, Lounges",
        },
    }

    // Accessing nested map elements
    fmt.Println("JFK Location: ", airports["JFK"]["Location"])
    fmt.Println("LHR Amenities: ", airports["LHR"]["Amenities"])

    // Updating a nested map element
    airports["JFK"]["Amenities"] = "Free WiFi, Lounges, Priority Boarding"
    fmt.Println("Updated JFK Amenities: ", airports["JFK"]["Amenities"])

    // Adding a new airport
    airports["NRT"] = map[string]string{
        "Location":  "Tokyo, Japan",
        "Amenities": "Restaurants, Duty-Free Shops",
    }
    fmt.Println("NRT Location: ", airports["NRT"]["Location"])
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

EXAMPLE 1 :
```go
package main

import "fmt"

func main() {
    // A map to handle airport codes
    airportCodes := map[string]map[string]string{
        "JFK": {"city": "New York", "country": "USA"},
        "LAX": {"city": "Los Angeles", "country": "USA"},
        "LHR": {"city": "London", "country": "UK"},
        "HND": {"city": "Tokyo", "country": "Japan"},
        "SYD": {"city": "Sydney", "country": "Australia"},
    }

    // TODO: Access and print the city and country that corresponds to a particular airport code.
    fmt.Println(airportCodes["LHR"]["city"] + " - " + airportCodes["LHR"]["country"])
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

EXAMPLE 2:
```go
package main

import "fmt"

func main() {
    // A map to handle airport codes
    airportCodes := map[string]map[string]string{
        "JFK": {"city": "New York", "country": "USA"},
        "LAX": {"city": "Los Angeles", "country": "USA"},
        "LHR": {"city": "London", "country": "UK"},
        "HND": {"city": "Tokyo", "country": "Japan"},
        "SYD": {"city": "Sydney", "country": "Australia"},
    }

    // TODO: Update 'country' of 'LHR' to 'England'
    airportCodes["LHR"]["country"] = "England"
    fmt.Println(airportCodes)
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


![img03](images/03.webp)

## Introduction to Control Structures in Go

Are you ready to dive deeper into Go? In this lesson, we will learn about **control structures**. **Control structures** are fundamental building blocks in programming that empower your code to take different actions based on various situations.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**What You'll Learn**

We'll be focusing on the `if` and `else` statements. These are the cornerstones of decision making in Go. To illustrate, suppose you want to travel, but your ability to do so depends on whether you have a passport. In programming terms, we model this real-world scenario as follows:
```go
package main

import "fmt"

func main() {

    hasPassport := true

    if hasPassport {
        fmt.Println("You are eligible to travel.")
    } else {
        fmt.Println("You cannot travel without a passport.")
    }
}
```
As you can see, the `if` statement checks whether the condition — in this case, having the passport being `true` is met. If so, the action within the `if` block, printing "You are eligible to travel," is executed. Otherwise, the code within the `else` block, which states "You cannot travel without a passport," is executed.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**A Note on Syntax**

In addition to understanding the `if` and else statements, mastering the syntax, particularly the use of braces `{}` that delineate blocks of code, is crucial. Braces, in combination with the `if` and `else` keywords, define the scope and block of instructions attached to each condition in Go.
```go
if passport {
    fmt.Println("You are eligible to travel.")  // This statement belongs to the if condition
    // Any additional code dependent on the passport being true would be placed here
} else {
    fmt.Println("You cannot travel without a passport.")  // This statement belongs to the else condition
    // Code to execute when the passport condition is false would be placed here
}
// Any code here would not be part of the if-else block and executes regardless of the passport condition
```
After each `if` or else statement, a block of code is enclosed within braces `{}` to introduce the instructions that should be executed if the condition is met. This syntax structure ensures your program can clearly follow which instructions belong to which condition, thereby facilitating an organized and error-free decision-making process.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Role of Parentheses in if Statements**

Unlike some other programming languages, Go does not require parentheses around the condition of an `if` statement. This helps make the code cleaner and easier to read.
```go
if hasPassport {
    fmt.Println("You are eligible to travel.")
}
```
However, if you prefer, you can still use them for clarity, though it is not a common practice in Go:
```go
if (hasPassport) {
    fmt.Println("You are eligible to travel.")
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Boolean Checks and Operators**

In Go, you can use both shorthand and full syntax for boolean checks. The shorthand simply uses the boolean variable itself:
```go
if hasPassport {
    fmt.Println("You can travel.")
}
```
Alternatively, you can compare the boolean variable with a boolean literal using equality/inequality operators. This is often used for clarity or when dealing with more complex boolean expressions:
```go
if hasPassport == true {
    fmt.Println("You can travel.")
} else if hasPassport != true {
    fmt.Println("You cannot travel.")
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Using "if" Without "else"**

In Go, an `if` statement can also be used independently, without the accompanying `else` block. This allows you to execute a block of code only when a certain condition is true, without needing to define alternate actions if the condition is false. This can be particularly useful when you only need to check for or handle specific cases. Here’s an example:
```go
package main

import "fmt"

func main() {
    temperature := 30

    if temperature > 25 {
        fmt.Println("It's a hot day!")  // This statement will execute only if the temperature is greater than 25
    }
    // No else block is used here, so nothing else is executed only if the temperature is not greater than 25
}
```
In this example, `"It's a hot day!"` is printed only if the temperature is greater than 25. If the condition isn't met, the program simply continues without executing any additional actions related to this condition. Keep in mind that any code following the `if` check is still executed, regardless of the condition state.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

EXAMPLE 1:
```go
package main

import "fmt"

func main() {
    // Declaring an initial balance of $4500
    balance := 4500

    // Checking if the Visa Application Balance is greater than or equal to 5000
    if balance >= 5000 {
        fmt.Println( "You are eligible to apply for a visa.")
    } else {
        fmt.Println("You must maintain a balance of at least $5,000 to apply for a visa.")
    }
}
```
&nbsp;&nbsp;&nbsp;

EXAMPLE 2:
```go
package main

import "fmt"

func main() {
    // Declaring a Boolean variable "hasPassport"
    hasPassport := true

    if hasPassport == true {
        fmt.Println("You are eligible to travel.")
    } else {
        fmt.Println("You cannot travel without a passport.")
    }
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

## Exploring Multiple Conditions with If-Else and Else If in Go

Are you ready to level up your programming skills? In the previous lesson on **control structures**, a foundation was laid. We now venture further into the world of handling multiple conditions.

**What You'll Learn**

In this unit, we will fortify our understanding of the `if` and `else` commands by incorporating another useful control structure called else `if`. The `else if` statement enables us to handle multiple conditions more flexibly in Go.

Before we dive in, here's a quick reminder on comparison operators used within conditionals: the `>` (greater than) and `<` (less than) operators are critical for comparing values, allowing the program to decide which path to follow based on the resultant Boolean (`true` or `false`) value.

Let us illustrate this using an example: suppose a travel agency offers different travel packages based on a user's `age`. A children's package is allocated to anyone below `18`, an adult's package is designated for those between `18` and `59`, and any individuals aged `60` and above receive a senior citizen's package.

Below is a code snippet that models this scenario using `if`, else `if`, and `else`:
```go
Go
Copy to clipboard
package main

import (
    "fmt"
)

func main() {
    age := 20 // Example age

    if age < 18 {
        fmt.Println("You are eligible for the children's travel package.")
    } else if age < 60 {
        fmt.Println("You are eligible for the adult's travel package.")
    } else {
        fmt.Println("You are eligible for the senior citizen's travel package.")
    }
}
```
As demonstrated, we can manage three distinct age conditions using the `else if` statement.


EXAMPLE :
```go 
package main

import (
    "fmt"
)

func main() {
    // TODO: Check age and assign a travel package accordingly
    age := 60  // Example age

    // TODO: Use an if statement to check if the age is less than 18
    if age < 18 {
        fmt.Println("You are eligible for the children's travel package.")
    // TODO: Use an else if statement to check if the age is less than 60
    } else if age <= 59 {
        fmt.Println("You are eligible for the adult's travel package.")
    // TODO: Use an else statement for individuals 60 and older
    } else {
        fmt.Println("You are eligible for the senior citizen's travel package.")
    }
}
 ```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Building on If-Else and Else If: Introducing Nested Conditions in Go

Are you excited to build upon our previous exploration of the `if`, `else if`, and else constructs in Go? As we've seen, these are powerful tools for decision-making. Now, we're taking a step further by delving into **nested conditions**. This concept allows us to introduce even more flexibility and sophistication into our programming logic.

&nbsp;&nbsp;&nbsp;

**What You'll Learn**

Nested conditions occur when an `if`,`else if`, or `else` construct is embedded within another `if`, `else if`, or `else` construct. By creating this hierarchy of conditions, we can effectively handle more complex decision-making processes.

Let's reconsider the travel agency example. Suppose that now, in addition to age, we also want to consider the customer's budget. People over the age of `18` with a budget above `$1000` receive an international travel package, while those with smaller budgets receive a local travel package. Here's how we can use **nested conditions** to model this in Go:
```go
package main

import "fmt"

func main() {
    age := 23
    budget := 1500

    if age > 18 {
        if budget > 1000 {
            fmt.Println("You are eligible for the international travel package.")
        } else {
            fmt.Println("You are eligible for the local travel package.")
        }
    } else {
        fmt.Println("You are eligible for the children's travel package.")
    }
}
```
As demonstrated, we formed a nested construct by placing an `if-else` condition inside another `if` condition. Similar to concentric structures, these **nested conditions** allow for managing intricate complexities in our code.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Combining Conditionals with Maps in Go

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Deepening Our Journey: Combining Conditionals with Data Structures

Remember our last exciting unit on **nested conditions** in Go? It was much like a thrilling ride, wasn't it? Now, it's time to up the ante on our knowledge quest. In this unit, we will uncover a vital concept: **combining conditionals with maps** in Go.

&nbsp;&nbsp;&nbsp;

**What You'll Learn**

&nbsp;&nbsp;&nbsp;

In Go, conditionals and data structures are not just separate elements. When they merge, your code gains enhanced depth and capability.

Imagine a scenario where a traveler is planning trips to multiple countries. In the previous unit, we discussed a traveler example. Now, envisage this traveler having a Go **map** of destinations containing key information on whether the traveler has visited a country.

```go
package main

import "fmt"

func main() {
    travelDestinations := map[string]map[string]string{
        "France": {"capital": "Paris", "visited": "no"},
        "Italy":  {"capital": "Rome", "visited": "yes"},
        "Spain":  {"capital": "Madrid", "visited": "no"},
    }

    destination := "France"

    if travelDestinations[destination]["visited"] == "yes" {
        fmt.Printf("You have already visited %s!\n", destination)
    } else {
        fmt.Printf("It seems you haven't visited %s yet. Get ready for an exciting adventure in %s!\n", 
                   destination, travelDestinations[destination]["capital"])
    }
}
```

As demonstrated, integrating conditionals with data structures like **maps** can significantly enhance the versatility and robustness of your code.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Understanding the Access Pattern for Nested Maps**

Before delving deeper, let's revisit a fundamental syntax frequently used: accessing nested map elements. When dealing with **maps** within **maps** (nested **maps**), as shown in our travel example, you will use` map[outerKey][nestedKey]` syntax to retrieve nested values.

&nbsp;&nbsp;&nbsp;

For instance, to obtain the capital of France from our `travelDestinations` **map**, you would write `travelDestinations["France"]["capital"]`. This first retrieves the **map** associated with `"France"` and then extracts the value for `"capital"` within that **map**. Always ensure the existence of the key in the outer **map** to avoid errors.


&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Why Combining Conditionals and Data Structures Matters**

Imagine crafting a strategy for complex decision-making processes. Based on various criteria, you may decide not just the next move but also systematically store that decision. When conditionals converge with data structures, they exponentially empower your Go code, unlocking a realm of exciting possibilities.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;



![img04](images/04.webp)

&nbsp;&nbsp;&nbsp;

## Introduction to for Loops in Go Programming

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Introduction to for Loops**

Are you ready to level up your Go programming skills? We are moving into more advanced techniques to take full control of Go's capabilities. This lesson focuses on the **for loop** — a versatile and powerful tool in Go that will enhance your coding efficiency tremendously.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Exploring for Loops**

In Go, a **for loop** allows us to execute a block of code a certain number of times or over a collection. It's incredibly useful for handling repetitive tasks without manually programming each repetition.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Classic "for" Loop Syntax**

The traditional syntax for a **for loop** in Go consists of three components: initialization, condition, and increment. This is typically used when you know in advance how many times you'd like the loop to run. Here's an example:

```go
package main

import "fmt"

func main() {
    for i := 0; i < 5; i++ {
        fmt.Println("Iteration", i)
    }
}
```
In this example:

- Initialization: `i := 0` sets up a loop variable `i` starting at `0`.

- Condition: `i < 5` continues the loop as long as `i` is less than `5`.

- Increment: `i++` increases `i` by `1` after each iteration.

Running this loop, you'd see:

```go
Iteration 0
Iteration 1
Iteration 2
Iteration 3
Iteration 4
```
This structure provides a clear and controlled way to run code a specified number of times.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Using for Loops with Collections**

**For loops** become even more powerful when combined with Go's capability to iterate over collections. For example, consider visiting each country in a list for a trip. Let's see what it looks like in Go:
```go
package main

import "fmt"

func main() {
    tripCountries := []string{"France", "Italy", "Spain", "Japan"}

    for _, country := range tripCountries {
        fmt.Println("Considering", country, "for the trip.")
    }
}
```
In this scenario, the loop iterates over `tripCountries`, with `range` used to specify the collection to iterate over. The loop assigns each element to the variable `country` during each iteration and executes the block of code with `fmt.Println`.



Running the loop, you would see:

```go
Considering France for the trip.
Considering Italy for the trip.
Considering Spain for the trip.
Considering Japan for the trip.
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example 1:
```go
package main

import "fmt"

func main() {
    // Suppose we have a list of countries we are considering for a trip
    tripCountries := []string{"France", "Italy", "Spain", "Japan"}

    // TODO: Identify the bug with this for loop preventing it from running correctly
    for _, country := range tripCountries {
        fmt.Printf("Considering %s for the trip.\n", country)
    }
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Discover the Power of Nested Loops in Go


I trust you're as excited as I am to delve further into this intriguing topic. Having established a solid foundation with for loops, it is now time to elevate your understanding of these looping concepts by exploring **nested loops**. **Nested loops** enable us to tackle more complex situations and enhance the power of our code.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Unfolding Nested Loops**

**Nested loops** signify the placement of one loop inside another. These loops prove particularly useful when we need to address scenarios involving more than one sequence or when the number of iterations depends on the data itself. The setup could involve a `for` loop inside another `for` loop, or even a combination of `for` loops and conditional logic.

Consider the following example. Suppose you're planning a trip and desire to list the key sights in the countries you intend to visit.

```go
package main

import "fmt"

func main() {
    // We might want to do some sightseeing in each country. For each country, we have a list of sights.
    countrySights := map[string][]string{
        "France": {"Eiffel Tower", "Louvre Museum"},
        "Italy":  {"Colosseum", "Piazza San Marco"},
        "Spain":  {"Park Güell", "The Alhambra"},
        "Japan":  {"Mt. Fuji", "Fushimi Inari Taisha"},
    }

    for country, sights := range countrySights {
        fmt.Printf("***In %s, I want to see:\n", country)
        for _, sight := range sights {
            fmt.Println(sight)
        }
    }
}
```
In this code snippet, we have a `for` loop that iterates over all countries, and within that loop, there is another `for` loop that cycles through all the sights `for` the current country. There you have it: a **nested loop!** Here is what you'd get if you run the code above:

```
***In France, I want to see:
Eiffel Tower
Louvre Museum
***In Italy, I want to see:
Colosseum
Piazza San Marco
***In Spain, I want to see:
Park Güell
The Alhambra
***In Japan, I want to see:
Mt. Fuji
Fushimi Inari Taisha
```

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


Example:
```go

package main

import (
    "fmt"
)

func main() {
    // Organizing a photo exhibition of iconic landmarks from around the world.
    exhibitionLandmarks := map[string][]string{
        "France": {"Louvre Museum", "Mont Saint Michel"},
        "Italy":  {"Leaning Tower of Pisa", "Venice Canals"},
        "Spain":  {"Sagrada Familia", "Royal Palace of Madrid"},
        "Japan":  {"Tokyo Tower", "Kiyomizu-dera"},
    }

    // TODO: Implement the loop to iterate through the countries and their landmarks
    for a, b := range exhibitionLandmarks {
        fmt.Printf("**countries who have landmarks: %s .\n", a)
        for _, c := range b {
            fmt.Println(c)
        }
    } 
}
```

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Refining Your Loop Skills

Let's take your understanding of **for loops** in Go to the next level by delving deeper into loop controls. You'll learn how to skillfully manage loop execution using the `break` and `continue` statements, which can be indispensable in crafting efficient code.

**Dive Deeper into Loop Controls**

Consider you're embarking on a journey and need to double-check your packing. Imagine realizing you've either forgotten essential items or mistakenly packed non-essentials. Loop controls in Go are your coding assistant here.

The `break` statement stops the loop prematurely, and the `continue` statement skips to the next iteration of the loop. Let's explore how you can use both effectively through a practical example of checking a packing list while avoiding unnecessary checks.

```Go
package main

import "fmt"

func main() {
    packingList := []string{"passport", "tickets", "camera", "clothes", "snacks"}
    packedItems := []string{"passport", "camera", "clothes", "snacks"}
    nonEssentialItems := map[string]bool{"snacks": true}
    
    forgetting := false
    for _, item := range packingList {
        if nonEssentialItems[item] {
            // Skip non-essential items
            continue
        }

        present := false
        for _, packedItem := range packedItems {
            if item == packedItem {
                present = true
                break
            }
        }
        
        if !present {
            fmt.Printf("Forgot to pack %s\n", item)
            forgetting = true
            // Stop checking further as we've already identified a missing item
            break
        }
    }
    
    if !forgetting {
        fmt.Println("All essential items packed!")
    }
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Explanation**

In this enhanced packing scenario, we iterate through the `packingList` but skip non-essential items using the `continue` statement if the item is found in the `nonEssentialItems` map. This ensures our check focuses solely on essentials.

The nested loop then verifies each essential item against the `packedItems`. If an item is missing, the break statement halts further iterations, signaling an oversight. Finally, the code provides feedback on whether all essential items are packed.

This approach, using both `break` and `continue`, optimizes the loop process: it avoids redundant checks and breaks out swiftly when an issue is detected, mimicking an intuitive checklist execution.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

Example : 
```go
package main

import "fmt"

func main() {
    gadgetList := []string{"smartphone", "camera", "power bank", "headphones"}
    packedGadgets := []string{"smartphone", "power bank", "headphones"}
    
    // TODO: Write a for loop to iterate through each gadget in gadgetList
    for _, item := range gadgetList {
        found := true
        // TODO: Inside the loop, write a nested loop to iterate through packedGadgets
        for _, packedGadget := range packedGadgets {
        // TODO: Use a boolean variable to check if the current gadget is in packedGadgets
            if item == packedGadget {
                found = false
                break
            }
        }
        // TODO: If the gadget is not in packedGadgets, print "Forgot to pack {gadget}" and use the break statement to exit the loop   
        if found {
            fmt.Printf("Forgot to pack %s. \n", item)
            break
        }    
    }
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Mastering Conditional Iteration with Go's for Loop

Welcome back! I hope you're prepared for another exciting lesson. Our journey through loops in Go continues. You've already built a strong foundation. Now, we'll enrich that knowledge by exploring how to use a conditional `for` loop in Go.

What Should You Expect?
Think of a `for` loop with a condition in Go as your reliable companion that continues performing a task as long as the condition remains true. It keeps iterating until the condition fails or a `break` statement intervenes. Let's illustrate how this works with a code snippet related to planning a trip based on the `budget` for the countries you want to visit:

```go
package main

import (
    "fmt"
)

func main() {
    // We have a budget for the trip, and each country costs a certain amount
    travelBudget := 5000
    countryCosts := map[string]int{"France": 1000, "Italy": 800, "Spain": 900, "Japan": 1200}

    totalCost := 0
    chosenCountries := []string{}

    // Let's add countries to our trip until our budget runs out using a for loop
    for totalCost < travelBudget && len(countryCosts) > 0 {
        // Iterate over map to get a country and its cost
        tried := 0
        for country, cost := range countryCosts {
            if totalCost+cost <= travelBudget {
                totalCost += cost
                chosenCountries = append(chosenCountries, country)
                delete(countryCosts, country)
            } else {
                tried++
            }
        }
        
        if len(countryCosts) == tried {
            // we don't have any more options that can fit within our budget so we can safely exist the loop
            break;
        }
    }

    fmt.Println("Countries chosen for the trip:", chosenCountries)
}
```
This code features a `for` loop that checks two conditions — whether our `totalCost` is less than our `travelBudget`, and whether there are still countries left in the countryCosts map. The loop continues iterating, removing countries from the map, and adding them to our trip until one of the conditions fails.

We are also monitoring how many countries we have checked within our loop. If it so happens that the count of countries checked matches the length of the collection of contries, we have exhausted our options and can exit our loop.

In this case, our budget permits travel to all the countries, so the loop stops when countryCosts is empty and there are no more countries to consider. If you adjust the travelBudget to a lower number, the loop will halt sooner when the first condition is no longer met.


Example :

```go
package main

import (
    "fmt"
)

func main() {
    tourBudget := 3500
    cityVisitCosts := map[string]int{"Rome": 850, "Paris": 900, "Berlin": 600, "Madrid": 700}

    totalSpent := 0
    citiesChosen := []string{}


    // TODO: Use a for loop to selectively add cities to the tour list based on the budget
    for totalSpent < tourBudget && len(cityVisitCosts) > 0 {
        
        tried := 0 
        
    // TODO: Retrieve a city and its associated cost  
        for city, cost := range cityVisitCosts {
    //	TODO: Check if adding this city would exceed your budget
            if totalSpent+cost <= tourBudget {
                totalSpent += cost
                citiesChosen = append(citiesChosen, city)
                delete(cityVisitCosts, city)
            } else {
                tried++
            }
        }
    
    
        // TODO: ensure that we can exit the loop if no more valid options are available
        if tried == len(cityVisitCosts) {
           break
        }
    }
    // TODO: Print the list of cities chosen for the cultural tour
    fmt.Println("List of cities chosen for the cultural tour:", citiesChosen)
    
    
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


![img05](images/05.webp)

## Diving Into Go Functions

Having introduced yourself to the fundamentals of programming, it's time to dive deeper. In today's lesson, we'll explore one of the most critical components of Go — **defining and using functions**. As you progress through this unit, you'll gain a comprehensive understanding of what **functions** entail, their syntax, and their application in Go.

**What is a Function and Its Syntax?**

A **function** is a block of code that performs a specific task independently. **Functions** help organize and reuse code effectively. You can think of a **function** as a small machine performing a designated task whenever it is invoked. Depending on its design, a **function** may receive some input and return an output after executing the necessary operations.

In Go, a **function** is defined using the `func` keyword, followed by the function's name and parentheses `()`. The code block of a **function** is enclosed within curly braces `{}`. Below is how you define a simple `function` in Go:

```go
package main

import "fmt"

func greetUser() {
    fmt.Println("Hello, traveler!")
}

func main() {
    greetUser()  // Outputs: Hello, traveler!
}
```
Here, we've implemented a **function** named `greetUser`. This **function** outputs a standard greeting: "Hello, traveler!". To utilize this **function**, it is called by its name within the `main` function.

**Recognizing the "main" Function**

Interestingly, you've already been using the function concept with the `main` function. In Go, the `main` function serves as the entry point for any standalone executable program. Go automatically invokes the `main` function when the program starts. This is akin to pressing the start button on a machine; the `main` function controls the flow of the program and orchestrates the execution of other functions.

**Why is Learning About Functions Important?**

Understanding **functions** is a significant milestone on your journey to becoming a proficient programmer. **Functions** are the building blocks of any software, helping structure your program into smaller, manageable segments. This organization makes your code more intuitive, reusable, and easy to understand.

Moreover, **functions** decrease the likelihood of errors and enhance the reliability of your code. With **functions**, you only need to update the function's implementation if there is a need to modify a part of the code. Any such update will automatically apply wherever the **function** is called.

---

---

```go
package main

import "fmt"

func main() {
    var planetsInSolarSystem int = 8 // Initially, we have 8 planets
    fmt.Println("Planets in our Solar System:", planetsInSolarSystem) // Prints: Planets in our Solar System: 8

    // TODO: Update the number of planets to 9 as if a new planet has been discovered.
    planetsInSolarSystem = 9
    fmt.Println("Updated Planet count:", planetsInSolarSystem) // Prints: Updated Planet count: 9
}
```

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Building upon Basic Functions with Function Parameters

Welcome back! We've laid the groundwork by defining basic **functions** in Go in the previous unit, haven't we? We discovered the crucial role of functions in structuring and reusing code, making our programs more readable and less prone to errors. We also crafted our very own Go function, greetUser, which provides a general greeting to any user. Today, we'll delve further into an important function feature — **function parameters**.

**Enhancing Function Functionality with Parameters**

Previously, our `greetUser` function issued a generic greeting that didn't distinguish between users. But what if we want to provide a more personalized greeting tailored to each user? This is where **function parameters** come into play.


**Parameters** allow functions to accept inputs, increasing their flexibility and versatility. You can introduce parameters into the function definition within parentheses. A parameter consists of a **name** and a **type**. The name is an identifier used within the function, while the type specifies what kind of data the parameter can hold. Let’s see the updated version of our greeting function.

```go
package main

import (
    "fmt"
)

// Define a function to greet the user by name
func greetUserByName(name string) {
    fmt.Printf("Hello, %s!\n", name)
}

// Main function to execute the greeting
func main() {
    greetUserByName("Alex")
}
```
In this example, our function `greetUserByName` now accepts the parameter `name` of type `string`. When we call the function and pass `"Alex"` as the argument, it outputs the greeting `"Hello, Alex!"` Wonderful, isn't it?

**Uncovering the Significance of Function Parameters**

**Function parameters** enable functions to adapt and handle various data inputs, making them reusable across different scenarios. By using parameters, a single **function** can perform operations with various inputs, thereby enhancing the flexibility and adaptability of your program.

A function can also have a list of parameters, separated by commas, allowing it to accept multiple inputs. For instance, you could define a function that takes a user's first and last name as separate parameters, enhancing the function's ability to handle complex data. This makes your functions even more dynamic, as they can perform operations involving several data pieces at once.

Here's a code snippet demonstrating the use of multiple parameters in a function:

```go
package main

import (
    "fmt"
)

// Define a function to greet the user using their first and last name
func greetUserFullName(firstName string, lastName string) {
    fmt.Printf("Hello, %s %s!\n", firstName, lastName)
}

// Main function to execute the greeting
func main() {
    greetUserFullName("Alex", "Johnson")
}
```
In this example, the function `greetUserFullName` takes two parameters, `firstName` and `lastName`, both of which are of type string. When we call this function with the arguments `"Alex"` and `"Johnson"`, it outputs the greeting `"Hello, Alex Johnson!"`. This snippet illustrates how multiple parameters can be used to provide more detailed input to a function.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Exploring Return Values in Go Functions

Hello once again! Great job mastering the concept of function parameters in the previous lesson; you are progressing really well. Do you recall our function `greetUserByName(name)`? That function took a parameter name and greeted the user by `name`. While this is a pretty handy function, it doesn't provide any usable output that we could further manipulate elsewhere in our code. In today's learning journey, we'll uncover the power of **return values** in functions that help us produce outputs for further use or calculations in our Go programs.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


**Understanding Return Values**

The **return value** is the result that our function produces. After execution, a function has the ability to give us a resulting value that can be used elsewhere in our code. We can assign this returned value to a variable or manipulate it according to our requirements.

Let's alter the function from the previous lesson to see how we can use return values:

```go
package main

import (
    "fmt"
)

// Define a function to greet the user by name
func greetUserByName(name string) string {
    return fmt.Sprintf("Hello, %s!\n", name)
}

// Main function to execute the greeting
func main() {
    greeting := greetUserByName("Alex")
    fmt.Println(greeting)
}
```
The function `greetUserByName` takes a string parameter `name` and returns a formatted greeting message using `fmt.Sprintf`. The `fmt.Sprintf` function in Go is part of the `fmt` package and is used to format and return a string without printing it. It takes a format string and a variable number of arguments, formatting them as specified and returning the resulting string. This function is useful for dynamically constructing complex strings and allows for flexibility in how output is generated and used. For example, `fmt.Sprintf("Hello, %s!", name)` formats the `name` variable into the string, producing a greeting message. In main, the function is called with "Alex", and the returned greeting is printed using `fmt.Println`. The use of a return value allows the `greeting` message to be stored in the greeting variable for further manipulation or display.

In Go, when a function has a single return type, it is specified directly after the parameter list in the function signature. This return type indicates the kind of value the function will produce and ensures that its output can be correctly handled or assigned in the code. In our example, the return type of the `greetUserByName` function is `string`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


**Returning Multiple Values**

Go functions have a powerful feature — the ability to return multiple values. This capability is handy when a function needs to return more than one piece of information.

Consider this simple example where we want a function to return both the sum and product of two numbers:

```Go
package main

import "fmt"

// Define a function to return sum and product.
func calculateSumAndProduct(a int, b int) (int, int) {
    return a + b, a * b
}

func main() {
    a, b := 5, 3

    // Call the function
    sum, product := calculateSumAndProduct(a, b)
    fmt.Printf("The sum is: %d\n", sum)
    fmt.Printf("The product is: %d\n", product)
}
```

Here, `calculateSumAndProduct` returns two integers: the sum and the product of `a` and `b`. These values are then assigned to `sum` and `product`.

In Go, functions can return multiple values, specified in parentheses and separated by commas in the function signature. This feature allows functions to simultaneously output several pieces of data, enhancing flexibility and efficiency by supporting operations like returning both a result and an error in a single call. In our example, the `calculateSumAndProduct` function returns 2 values of type `int` and `int`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Named Return Values**

Go also allows naming return values in the function signature, which can make the code clearer by describing what each returned value represents. Here's our previous example modified to use named return values:
```go
package main

import "fmt"

// Define a function with named return values.
func calculateValues(a int, b int) (sum int, product int) {
    sum = a + b
    product = a * b
    return
}

func main() {
    a, b := 5, 3

    // Call the function
    sum, product := calculateValues(a, b)
    fmt.Printf("The sum is: %d\n", sum)
    fmt.Printf("The product is: %d\n", product)
}
```
By using named return values (`sum` and `product`), the code becomes self-documenting, clarifying the purpose of each return value and potentially reducing errors caused by misinterpretation.

**Why Return Values are Important**

You might be pondering why return values are so crucial. Well, **return values** essentially transform our functions into data factories - they process data and then deliver it for us to use. This practice makes our code modular, flexible, and efficient, as we can reuse functions to create different outputs based on our input data.

Understanding **return values** deepens your mastery of functions and presents a world of programming possibilities. With **return values**, your functions aren't simply processing tasks - they're creating something for you to use elsewhere in your program, saving you repetitive work and making your code cleaner and more efficient.



example1 :

```go
package main

import "fmt"

// TODO: Add a second return value of type bool
func calculateDivision(a float64, b float64) (float64, bool) {
    // TODO: Check if b is zero
    if b == 0 {
    //  return 0 and true if so
        return 0, true
    }
    //  return the division result and false if not
    return a/b, false
}

func main() {
    var a, b float64 = 5, 0

    // TODO: capture both values from the function
    division, isError := calculateDivision(a, b)
    // TODO: check if we were dividing by zero
    if isError {
        //  if so, write to the console that this isn't allowed
        fmt.Printf("Division by zero isn't allowed")
        } else { 
        //  otherwise print out the result of the division
        fmt.Println("The result of the division is:", division)
        }

}
```

example2:

```go
package main

import "fmt"

// Define a function to calculate the transportation cost.
func calculateTransportationCost(countries []string, transportationCosts map[string]int) int {
    totalCost := 0
    for _, country := range countries {
        totalCost += transportationCosts[country]
    }
    // TODO: Return the total cost
    return totalCost
}
func main() {
    // Presuming the countries you'll visit and the expected transportation costs for each
    plannedCountries := []string{"Germany", "Austria", "Switzerland"}
    expectedCosts := map[string]int{"Germany": 200, "Austria": 180, "Switzerland": 220}

    // TODO: Call the function and assign return value to transportationCost
    transportationCost := calculateTransportationCost(plannedCountries, expectedCosts)
    fmt.Printf("The total transportation cost for the trip is: $%d\n", transportationCost)
}
```

example 3 :

```go
package main

import (
	"fmt"
)

// TODO: Define a function called calculateSouvenirBudget
// TODO: The function should take two parameters: countries (a slice of countries) and souvenirCosts (a map with countries as keys and costs as values)
func calculateSouvenirBudget(counteries []string, souvenirCosts map[string]int) int{
// TODO: Inside the function, create a variable to hold the total budget and set it to 0
    totalbudget := 0
// TODO: Use a for loop to iterate through the slice of countries
    for _, country := range counteries {        
// TODO: For each country, add the corresponding souvenir cost to the total budget
        totalbudget += souvenirCosts[country]
    }
// TODO: The function should return the total budget
        return totalbudget
}

func main() {
    // Assuming the countries you'll visit and the average souvenir costs
    countries := []string{"France", "Italy", "Spain"}
    souvenirCosts := map[string]int{"France": 150, "Italy": 100, "Spain": 75}

    // Call the function
    totalSouvenirBudget := calculateSouvenirBudget(countries, souvenirCosts)
    fmt.Printf("The total souvenir budget for the trip is: $%d\n", totalSouvenirBudget)
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


#### Introduction to Variable Scope: Local and Package-Level Variables

Welcome back! We are advancing swiftly to another significant terrain: **variable scope** in Go. You've already learned how to create and call functions, as well as how to incorporate return statements. Now, we move to one of the crucial aspects of functions — understanding the scope of variables both within and outside of these functions. Are you thrilled to dive in? We guarantee it's going to be enlightening!



**Understanding Local and Package-Level Variables**

In Go, a variable defined within a function has a scope confined to that function, making it a local variable. This simply means that you cannot access a local variable outside the function in which it's declared.

What happens if we want a variable that is accessible across functions within the same package? That's where package-level variables come in! Package-level variables are those defined outside any function and are accessible throughout your code — both inside and outside functions within the same package.

Let's step through an example to illustrate:

```go
package main

import "fmt"

// Define a package-level variable
var chosenCountries = []string{"France", "Italy"}

func addCountry(country string) {
    chosenCountries = append(chosenCountries, country)  // This modifies the package-level variable
}

func main() {
    addCountry("Spain") // Invoke the function
    fmt.Println(chosenCountries) // Output: ["France", "Italy", "Spain"]
}
```
Here, chosenCountries is a package-level variable. We are able to append a new country to our slice within the function `addCountry()`. After invoking a`ddCountry()` with "Spain," we printed `chosenCountries` and found its value to be `["France", "Italy", "Spain"]`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Trying to Access a Variable Not in Scope**

Attempting to access a variable that is not within your current scope is a common mistake. This occurs when you try to access a local variable outside the function in which it is defined.

Consider this example:
```go
package main

import "fmt"

func bookFlight() {
    destination := "Paris"  // Local variable defined within the function
}

func main() {
    bookFlight()
    fmt.Println(destination)  // Attempt to access the local variable outside its function
}
```
Running this code will result in a compilation error because `destination` is not declared in the package scope. Go enforces scope rules to maintain clarity and prevent unexpected alterations to data.



Example 
```go
package main

import "fmt"

// TODO: Declare a package-level slice to keep track of visited landmarks
var landmarks []string 
// TODO: Define a function named logLandmark that takes two parameters: landmark and city
func logLandmark(landmark string, city string) {
    // TODO: Add the landmark and its city to the package-level slice in the format "landmark in city"
    entry:= fmt.Sprintf("%s in %s", landmark, city) 
    landmarks = append(landmarks, entry)
}
    

func main() {
    // TODO: Call the logLandmark function with examples e.g. "Eiffel Tower" and "Paris", "Statue of Liberty" and "New York"
    logLandmark("Eiffel Tower", "Paris")
	logLandmark("Statue of Liberty", "New York")


    // TODO: Print the slice of visited landmarks
    fmt.Println(landmarks)
    
}
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Combining Functions to Solve Complex Problems

Welcome back, Go enthusiast! In our last lesson, we successfully navigated the intricacies of **variable scope** in Go. Today, we're elevating our skills. We'll learn how to combine multiple functions to tackle more complex problems. Picture it as constructing a multi-star engineer's toolkit; each tool is a function, and employing the right combination can assist you in crafting an architectural masterpiece.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

**Breaking Down Problems using Functions**

Imagine embarking on a European trip; you need to compute the cost of your visits to various countries based on a budget. It seems daunting, doesn't it? But fear not; with Go functions, we can dissect this problem into smaller, more manageable tasks.

Before diving into function creation, let's consider some sample data to work with:

``Go
package main

import "fmt"

var travelBudget = 5000
var countryCosts = map[string]int{"France": 1200, "Italy": 1500, "Spain": 800, "Germany": 900, "Greece": 1100}
```
Here, we have a travel budget of `5000`, and the costs of visiting France, Italy, Spain, Germany, and Greece are outlined in our **map**, `countryCosts`. These values will serve as the basis for crafting and demonstrating our functions.


**Selecting Countries within Budget**

First, let's focus on selecting which countries we can visit without exceeding our budget using the `chooseCountries` function:
```go
func chooseCountries(budget int, costs map[string]int) []string {
    totalCost := 0
    chosenCountries := []string{}
    for country, cost := range costs {
        if totalCost+cost > budget {
            break
        }
        totalCost += cost
        chosenCountries = append(chosenCountries, country)
    }
    return chosenCountries
}

func main() {
    chosenCountries := chooseCountries(travelBudget, countryCosts)
    fmt.Println(chosenCountries) // Prints [France Italy Spain Germany]
}
```
This function iterates through each country and its associated cost, adding countries to our visit list until we reach our budget limit. This direct approach ensures that our travel experience is always within budgetary constraints.


**Calculating the Total Cost**

Next, we will calculate the total cost of visiting the selected countries with the calculateCost function:
```go
func calculateCost(countries []string, costs map[string]int) int {
    totalCost := 0
    for _, country := range countries {
        totalCost += costs[country]
    }
    return totalCost
}

func main() {
    chosenCountries := chooseCountries(travelBudget, countryCosts)
    totalCost := calculateCost(chosenCountries, countryCosts)
    fmt.Println(totalCost) // Prints 4400
}
```
After selecting our countries, this function calculates the total expenditure. By iterating through the list of chosen countries, it sums up their associated costs from our **map**, `countryCosts`, providing us with a clear tally of our planned expenses.



example :
```go
package main

import "fmt"

// Partial function to add another country to your trip plan if it fits the budget.
func addCountryIfFitsBudget(currentBudget int, currentCost int, newCountryCost int) bool {
    // TODO: Add the missing code to check if you can add the new country without exceeding the budget
    totalCost := currentCost + newCountryCost
    if totalCost <= currentBudget {
        return true
    }
    return false
}

func main() {
    // Assuming sample values for demonstration
    currentBudget := 5000
    currentCost := 3000
    newCountryCost := 1200

    // Check if adding the new country fits the budget
    canAddCountry := addCountryIfFitsBudget(currentBudget, currentCost, newCountryCost)
    fmt.Printf("Can add new country within budget? %v\n", canAddCountry)
}
```


example:
```go
package main

import "fmt"

// Adjusted function to consider both budget and cost preference.
func chooseCountries(budget int, costThreshold int, costs map[string]int) []string {
    totalCost := 0
    chosenCountries := []string{}
    for country, cost := range costs {
        // TODO: Modify the condition below to also reject countries whose cost exceeds the costThreshold
        if totalCost+cost > budget {
            continue
        }
        if cost > costThreshold {
            continue
        }
        totalCost += cost
        chosenCountries = append(chosenCountries, country)
    }
    return chosenCountries
}

func calculateCost(countries []string, costs map[string]int) int {
    totalCost := 0
    for _, country := range countries {
        totalCost += costs[country]
    }
    return totalCost
}

func main() {
    // Assuming sample data for budget, cost threshold, and costs for demonstration
    travelBudget := 5000
    costThreshold := 1000 // New cost preference limit
    countryCosts := map[string]int{"France": 1200, "Italy": 1500, "Spain": 800, "Germany": 900, "Greece": 1100}

    chosenCountries := chooseCountries(travelBudget, costThreshold, countryCosts)
    tripCost := calculateCost(chosenCountries, countryCosts)

    fmt.Printf("The countries selected within budget and cost preference are: %v\n", chosenCountries)
    fmt.Printf("The total cost of the trip is: $%d\n", tripCost)
}
```
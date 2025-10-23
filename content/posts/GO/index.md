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

## Introduction to Programming with Go

### Taking Your First Step in Go Programming

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

#### Understanding the Code

The code snippet contains some elements that we will discuss briefly:

- `package main`: Since your program is meant to run, it must be named `main`. We are focusing on running our program so for now just know that this is the name that must be used in order to run the program.

- `import "fmt"`: We are printing something as output on the screen, and the built-in package `fmt` gives us access to this functionality.

- `func main()`: This defines the `main` function, which is the entry point of every executable Go program.

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

#### Managing Quantities with Variables

Are you eager to expand your knowledge of Go programming? Fantastic! In this lesson, we'll focus on using `variables` in Go to manage quantities. This is a crucial feature for organizing data and performing computations. Go emphasizes type safety, ensuring that the data you work with is consistent and reliable.

#### Getting Into The Nitty-Gritty

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

#### What You'll Learn

Are you ready to explore combining text with variables in Go using `fmt.Sprintf`? This versatile feature makes it effortless to create dynamic and readable strings by embedding variables directly within them.

#### Code Example

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


#### Introduction to Go Variables and Booleans

An exciting lesson awaits us, promising a deeper exploration of Go's **variables** and **Boolean types**. In earlier lessons, we covered the basics of Go, and now we're going to build on that by exploring how to use **Boolean variables**. These are simple yet powerful, used to represent a condition or status as either `true` or `false`.

#### What You'll Learn

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

## Introduction to Slices in Go

### Introduction to Creating and Accessing Slices

Welcome to an exciting journey in the world of programming with Go.

Today, we will explore one of Go's key features: data structures, specifically **slices**. **Slices** are an integral part of Go, providing the functionality you need to manage collections of data efficiently. Let's dive into the world of **slices**!

#### Packing Our Travel Bags with Slices!

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

#### Accessing the Elements of Travel

Accessing elements from a **slice** is like selecting a travel destination from your list. Go uses a zero-index system, meaning the first element within a **slice** is accessible via the index `0`. Let's see how you can access different elements from our **slice**.

To access an element from the **slice**, you refer to its index. For instance, if you want the first destination on our list, which is "Paris," you can do the following:

&nbsp;&nbsp;&nbsp;

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

&nbsp;&nbsp;&nbsp;

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

### Adding and Removing Items from Slices in Go

Welcome back, traveler! As part of our travel-themed journey, we'll be managing a slice of countries for our hypothetical world tour! Just as it is in real-world travel, our itinerary may change, prompting us to add or remove countries from our slice. In Go, slices provide a powerful way to work with collections of elements. Today, we'll explore how to append new items and manually remove items from slices.

#### What You'll Learn

Let's learn how to manipulate slices in Go, focusing on how to add and remove items. We will use the built-in function `append()`, which is used to add an item to the end of a slice. For removing items, we'll demonstrate slicing techniques to manually handle the removal of elements.

#### Slicing with Indexes in Go

Before we cover modifying slices, let's talk about slicing slices! By using slicing syntax, you can create a new slice based on the elements between specified indexes of an existing slice. Here's how slice indexing works:

1. **Basic Slicing Syntax**:

The slicing operation is performed using a colon : inside square brackets to specify the start and end indexes. The syntax follows the format `slice[startIndex:endIndex]`.

- **Start Index**: Specifies the index at which the new slice begins (inclusive).

- **End Index**: Specifies the index at which the new slice ends (exclusive). The element at endIndex is not included in the new slice.

2. **Omitting Indexes**:

- If the start index is omitted, it defaults to `0`, i.e., the beginning of the original slice.

- If the end index is omitted, it defaults to the length of the original slice, i.e., the end of the original slice.

```go
fromStart := numbers[:4]  // Creates a new slice with elements {0, 1, 2, 3}
toEnd := numbers[6:]      // Creates a new slice with elements {6, 7, 8, 9}
entireSlice := numbers[:] // Creates a copy of the entire slice {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
```
Slicing with indexes is an essential feature in Go that provides flexibility in accessing and manipulating collections of data efficiently. By understanding how to define start and end points, you can leverage slices to perform complex data operations seamlessly.

#### Using the "append" Function

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

&nbsp;&nbsp;&nbsp;

#### Exploring Arrays in Go

Welcome to our continuing adventure through Go's data structures! In this lesson, we'll shift our focus from slices to **arrays**. Let's dive in!

&nbsp;&nbsp;&nbsp;

#### Creating an Array

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

&nbsp;&nbsp;&nbsp;

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

### Welcoming Maps

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

### Updating an Existing Value

To update an existing value in a **map**, use the key to assign a new value.

```Go
// Updating the capital of France
capitalCities["France"] = "Marseille"

// Verify the update
fmt.Println("The updated capital of France is:", capitalCities["France"])  // Output: The updated capital of France is: Marseille
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Removing a Value

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
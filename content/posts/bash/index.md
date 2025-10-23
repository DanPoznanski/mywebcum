---
title: "Bash"
discription: Bash
date: 2025-09-18T21:29:01+08:00 
draft: false
type: post
tags: ["Bash","Scripts","Linux"]
showTableOfContents: true
---

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;




&nbsp;&nbsp;&nbsp;


![img](images/img1.png)

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;





&nbsp;&nbsp;&nbsp;

## Using Variables in Shell Scripts
Welcome back to the world of shell scripting. In the previous lesson, you crafted your very first shell script that printed "Hello, World!" to the screen. Today, we will take the next step by exploring how to use **variables** in shell scripts. This lesson will introduce you to variable declaration, reassignment, and some basic arithmetic operations.

Using variables can make your scripts more dynamic and flexible, allowing you to store information, reuse values, and perform calculations. Let's get started!

### Creating and Using Variables
Variables in shell scripts are used to store data that can be reused throughout the script. To define a variable in a shell script, you use the syntax:

```bash
variable_name=value
```
An important distinction of bash is there **cannot** be spaces before or after the `=` sign

To access the value of a variable, use a `$` before the variable name.

```bash
#!/bin/bash

greeting="Hello"
name="World"
echo "$greeting, $name!"  # Prints: Hello, World!
```
- `greeting="Hello"`: This line creates a variable named `greeting` and sets its value to "Hello."

- `name="World"`: Another variable named `name` is created and assigned the value "World."

- `echo "$greeting, $name!"`: This command prints the variables `greeting` and `name` with a comma and exclamation mark. The output will be: `Hello, World!`

Variables are essential for making your scripts more modular and easier to maintain. The values can be changed without modifying the entire script.

&nbsp;&nbsp;&nbsp;

### Variable Reassignment

Variables in shell scripts can be reassigned new values, making them adaptable to different scenarios. Let’s see how variable reassignment works:

```bash
#!/bin/bash

greeting="Hello"
hello=$greeting
echo $hello  # Prints: Hello
```
- `hello=$greeting`: This line assigns the value of the variable `greeting` to the variable `hello`. The `$` is required to access the value stored in the `greeting` variable

- `echo $hello`: This prints the value of `hello`, which is now "Hello."

With variable reassignment, you can easily propagate changes throughout your script by modifying a single value.

&nbsp;&nbsp;&nbsp;

### Defining and Using Integers

Shell scripts also allow you use arithmetic operations such as `+`, `-`, `*`, `/`, and `%`. Variables used in arithmetic operations must be preceded by `$`. The `$(())` syntax is used to evaluate the arithmetic expression within the parentheses. Let's take a look:

```bash
#!/bin/bash

# Defining integers
num1=2
num2=5

# Performing addition 
sum=$(($num1 + $num2))
echo $sum  # Prints: 7
```
- `num1=2` and `num2=5`: These lines create two integer variables, `num1` and `num2`.

- `sum=$(($num1 + $num2))`: This line access `num1` and `num2` using `$`. The arithmetic expression is inside `$(())` to correctly evaluate the expression.

&nbsp;&nbsp;&nbsp;

### Common Arithmetic Pitfalls

When performing arithmetic operations in shell scripts, it's easy to run into some common pitfalls. Let's take a look at a couple of examples that illustrate mistakes to avoid:

```bash
#!/bin/bash

num1=2
num2=5

x=num1+num2
echo $x  # Prints: "num1+num2"

y=$num1+$num2
echo $y  # Prints: "2+5"
```
- `x=num1+num2`: This line attempts to set the value of `x` as the sum of `num1` and `num2`, but it actually assigns the string "num1+num2" to `x`. This is incorrect because the variable names are treated as a plain strings.

- `y=$num1+$num2`: This line tries to concatenate the values of `num1` and `num2`. Instead of adding the numbers, it results in the string "2+5". This happens because when using the `$` syntax without the `$(())`, the shell does not perform arithmetic operations.

To perform arithmetic operations correctly, always use the `$(())` syntax,

&nbsp;&nbsp;&nbsp;

### Correctly Naming Variables

When naming variables in shell scripts, it's important to follow certain rules to ensure your script runs correctly. Here are the key rules for proper variable naming:

- Variable names must begin with a letter (a-z, A-Z) or an underscore (`_`).

- After the first character, variable names can include letters, numbers (0-9), and underscores, but no other special characters.

- Variable names cannot contain spaces or special characters like `!`, `@`, `#`, `$`, `%`, `^`, `&`, `*`, `(`, `)`, `-`, `+`, `=`, etc.

- Variable names are case-sensitive. For example, Var and var are considered different variables.

- Do not use reserved words or keywords (such as `if`, `then`, `else`, `fi`, `for`, `while`, etc.) as variable names, as they have special meanings in shell scripting.

Proper variable naming is crucial to avoid errors and improve code readability. Let’s see some correct and incorrect ways to name variables:

```bash
#!/bin/bash

# Correctly naming variables
var=1
var1=1
_var_1=1

# Incorrectly naming variables (commented out to avoid errors)
# 1Var=1
# 123=1
# !var=1
# @var=1
# var*1=1
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

## Comparison Operators in Shell Scripting

Introduction to Comparison Operators
Welcome to another exciting lesson in your shell scripting journey. In our previous lesson, we delved into using variables in shell scripts — learning how to store data that can be reused throughout a script. Today, we will build on that foundation by exploring **Comparison Operators** in shell scripting.

Comparison operators are essential tools for making decisions within your scripts. They allow you to compare values and execute different actions based on the results of these comparisons. This lesson will cover numeric and string comparisons and help you understand how to incorporate these comparisons into your scripts.

Let's dive in!

&nbsp;&nbsp;&nbsp;

### Explanation of Exit Status

Before diving into comparison operators, let's learn about exit statuses. An exit status is a numeric value returned by a command or a script to indicate its execution result. In the context of shell scripting, every command executed returns an exit status, which can be captured and used to determine the success or failure of that command.

An exit status of `0` indicates that the command or script was successful. A non-zero exit status (usually `1`) indicates that the command or script failed.

In shell scripting, comparisons return an exit status to indicate the result of the comparison

- `0`: Indicates the comparison is true.

- `1`: Indicates the comparison is false.

The `echo $?` command in shell scripting is used to print the exit status of the last executed command

&nbsp;&nbsp;&nbsp;

### Numeric Comparisons

Numeric comparisons are used to evaluate the relationship between two numbers. In shell scripting, we use the `[]` syntax with specific operators to perform these comparisons. There must be a single space at the beginning and end of the condition. Here are the most common numeric comparison operators:

- `-eq`: Equal to

- `-ne`: Not equal to

- `-gt`: Greater than

- `-lt`: Less than

- `-ge`: Greater than or equal to

- `-le`: Less than or equal to

Let's see an example:
```bash
#!/bin/bash

# Variables
num1=10
num2=20

# Equal to
[ $num1 -eq $num2 ]
echo $?  # 1 (false)

# Not equal
[ $num1 -ne $num2 ]
echo $?  # 0 (true)

# Greater than
[ $num1 -gt $num2 ]
echo $?  # 1 (false)

# Less than
[ $num1 -lt $num2 ]
echo $?  # 0 (true)

# Greater than or equal to
[ $num1 -ge $num2 ]
echo $?  # 1 (false)

# Less than or equal to
[ $num1 -le $num2 ]
echo $?  # 0 (true)
```

- `num1=10` and `num2=20` defines two integer variables

- For each comparison (`-eq`, `-ne`, `-gt`, `-lt`, `-ge`, `-le`), the result is checked, and the exit status is printed using `echo $?`.

- `echo $?` prints `0` for true and `1` for false.

Understanding these operators lets you incorporate numeric conditions into your scripts effectively.

&nbsp;&nbsp;&nbsp;

### String Comparisons

Just like numbers, string comparisons help you evaluate relationships between text values. Here are the most common string comparison operators:

- `==` or `=`: Equal to

- `!=`: Not equal to

- `-z`: String is null (has a length of zero)

- `-n`: String is not null (has a length greater than zero)

Let's explore an example:

```Bash
#!/bin/bash

# Variables
str1="Hello"
str2="World"
str3="Hello"

# Equal to with ==
[ "$str1" == "$str2" ]
echo $?  # 1 (false)

# Equal to with =
[ "$str1" = "$str3" ]
echo $?  # 0 (true)

# Not equal to
[ "$str1" != "$str2" ]
echo $?  # 0 (true)

# Null
[ -z "$str1" ]
echo $?  # 1 (false)

# Not null
[ -n "$str1" ]
echo $?  # 0 (true)
```

- `str1="Hello"`, `str2="World"`, and `str3="Hello"` Define three string variables.

- Various string comparisons are performed, and the result is checked using `echo $?`.

- The exit status indicates whether the comparison is true or false.

These comparisons help you manipulate and evaluate text in your scripts.

&nbsp;&nbsp;&nbsp;

### Quotes in String Comparisons

Using quotes in string comparisons like `[ "$str1" == "$str2" ]` is essential for several important reasons:

#### Handling Empty Strings

Quotes help manage cases where variables might be empty or undefined. Without quotes, an empty variable can lead to syntax errors:

```bash
#!/bin/bash

str1=""
[ $str1 == "Hello" ]  # [ == "Hello" ]
```
In this case, the shell interprets `str1` as an empty value, leading to an incomplete comparison like `[ == "Hello" ]`, which is not valid syntax.

#### Preserving Whitespaces

Quotes ensure that any whitespaces within string values are preserved and treated correctly. If you compare strings without quotes, the shell may treat each whitespace-separated part of the string as a distinct argument.

```bash
#!/bin/bash

str1="Hello World"
[ $str1 == "Hello World" ]  # [ Hello == Hello World ]

```
Here, `str1` is split into two arguments: Hello and World. The comparison [ Hello == Hello World ] is not valid syntax and will lead to an error.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

## Control Structures and Logical Operators in Shell Scripting

&nbsp;&nbsp;&nbsp;

### Introduction to Control Structures and Logical Operators

In this lesson, we will delve into an essential aspect of shell scripting — **Control Structures and Logical Operators**. Control structures such as `if`, `elif`, and `else` allow you to make decisions within your scripts, enabling them to respond intelligently based on different conditions. Additionally, logical operators like `&&` (AND) and `||` (OR) help you combine multiple conditions to create more complex logic.

Let's get started!

&nbsp;&nbsp;&nbsp;

### Syntax of if/elif/else Statements

The `if/elif/else` control structure allows you to perform different actions based on various conditions. Here’s the syntax:

```bash
if [ condition1 ]
then
    # Commands to execute if condition1 is true
elif [ condition2 ]
then
    # Commands to execute if condition1 is false and condition2 is true
else
    # Commands to execute if both condition1 and condition2 are false
fi
```
Each condition must be enclosed in `[]`. After the condition, the then keyword specifies the code to execute if the condition evaluates to `0` (true).

All control structures must start with an `if` statement. The `elif` and `else` statements are optional. Each `elif` condition is checked until one of the conditions evaluates to true. `If` none of the `if` or `elif` conditions are true, the `else` statement is executed.

The control structure must end with `fi`.

&nbsp;&nbsp;&nbsp;

### Using If-Else Statements

Let's take a look at an example to determine if a number is positive, negative, or neither.

```bash
#!/bin/bash
# Script to demonstrate if-else control structure
number=5

if [ $number -gt 0 ]
then
   echo "The number is positive"  # Prints: "The number is positive"
elif [ $number -lt 0 ]
then
   echo "The number is negative"  # This won't execute
else
   echo "The number is zero"  # This won't execute
fi
```
- `number=5` initializes the variable `number` with the value 5.

- The if statement checks if `number` is greater than 0.

- If true, it prints "The number is positive".

- If the first condition is false, the `elif` statement checks `if` number is less than 0.

- If the `elif` condition is also false, the `else` statement executes, printing "The number is zero".

&nbsp;&nbsp;&nbsp;

### Introducing Logical Operators

Logical operators allow you to combine multiple conditions in your control structures. The `&&` (AND) operator ensures both conditions must be true, while the `||` (OR) operator requires at least one condition to be true.

Let's examine how they work:

#### Using the && Operator

The `&&` (AND) operator is used to combine multiple conditions in a control structure such that all the conditions must be true for the block of commands to execute. For example:

```bash
#!/bin/bash
# Variables
num1=10
num2=20
str1="Hello"
str2="World"

# Using && (AND) Operator
if [ $num1 -lt $num2 ] && [ "$str1" != "$str2" ]
then
    echo "num1 is less than num2 AND str1 is not equal to str2"  # Prints: "num1 is less than num2 AND str1 is not equal to str2"
else
    echo "Condition is false"  # This won't execute
fi
```
- `num1` is 10, `num2` is 20, `str1` is "Hello", and `str2` is "World".

- The if statement checks if `num1` is less than `num2` **AND** `str1` is not equal to `str2`.

- Both conditions are true, so the script prints: `"num1 is less than num2 AND str1 is not equal to str2"`.

&nbsp;&nbsp;&nbsp;

### Using the || Operator

The `||` (OR) operator is used to combine multiple conditions in a control structure such that at least one of the conditions must be true for the block of commands to execute. For example:

```bash
#!/bin/bash

# Variables
num1=10
num2=20

# Using || (OR) Operator
if [ $num1 -gt 5 ] || [ $num2 -lt 5 ]
then
    echo "num1 is greater than 5 OR num2 is less than 5"  # Prints: "num1 is greater than 5 OR num2 is less than 5"
else
    echo "Neither condition is true"  # This won't execute
fi
```

- The `if` statement checks if `num1` is greater than 5 **OR** `num2` is less than 5.

- Since `num1` is indeed greater than 5, the script prints: `"num1 is greater than 5 OR num2 is less than 5"`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

## Arrays and Looping Constructs in Shell Scripting

&nbsp;&nbsp;&nbsp;

### Introduction to Arrays and Looping Constructs in Shell Scripting

Hello! In this lesson, we will explore the powerful concepts of **arrays** and **looping constructs** in shell scripting. These are essential tools that will help you manage and manipulate data efficiently within your scripts. By the end of this lesson, you'll be able to handle arrays, iterate over their elements, and utilize different looping constructs to automate repetitive tasks.

Arrays allow you to store multiple items in a single variable, making it easier to handle lists of data. Looping constructs such as `for` and `while` loops enable you to execute a block of code multiple times, adding flexibility and power to your scripts.

Let's start our journey into arrays and looping constructs!

&nbsp;&nbsp;&nbsp;

### Syntax for Creating Arrays

In shell scripting, creating an array involves declaring a variable and assigning it multiple values enclosed in parentheses. Each value is separated by a space. Here’s the general syntax:

```bash
#!/bin/bash
computers=("Dell" "HP" "Lenovo")
```
You can add elements to an array using `+=`. For example:

```bash
#!/bin/bash
computers=("Dell" "HP" "Lenovo")
computers+=("Mac")
```
Now the array also includes the value "Mac".

&nbsp;&nbsp;&nbsp;

### Array Length and Printing

Often you need to find the length of an array or print out all of its contents. You can access the length of an array using `${#array_name[@]}`. To print the contents of an array use `${array_name[@]}`. Let's take a look:

```bash
#!/bin/bash
computers=("Dell" "HP" "Lenovo")
computers+=("Mac")
echo "Number of computers: ${#computers[@]}"
echo "All computers: ${computers[@]}"

```
- `${#computers[@]}` accesses the number of elements using the `${#array_name[@]}` syntax

- `${computers[@]}` accesses all the elements of the array using the `${array_name[@]}` syntax.

This script prints out
```
Number of computers: 4
All computers: Dell HP Lenovo Mac
```
&nbsp;&nbsp;&nbsp;

### Array Indexing

Array indexing is the method of accessing individual elements in an array using their position, known as the index. Similar to other coding languages, the first element is at index 0. To access an element of an array use `${array_name[index_number]}`. Let's take a look:

```bash
#!/bin/bash
computers=("Dell" "HP" "Lenovo")
echo "The first computer is: ${computers[0]}" 
echo "The second computer is: ${computers[1]}" 
echo "The third computer is: ${computers[2]}"
```

The output of this script is:

```bash
"The first computer is: Dell"
"The second computer is: HP"
"The third computer is: Lenovo"
```
With this understanding of arrays, let's shift our focus to loops.

### Using While Loops

The `while` loop in shell scripting allows you to execute a block of code repeatedly as long as a given condition is true. It’s useful for cases where the number of iterations is not known beforehand and depends on certain conditions. The syntax structure of a `while` loop is:

```bash
#!/bin/bash
while [ condition ]
do
    command1
    command2
    ...
done
```
- `condition` is a test expression that is evaluated before each iteration. The loop continues as long as this condition is true.

- The `do...done` block contains the commands to be executed as long as the condition is true.

Let's look at an example:

```bash
#!/bin/bash

counter=1

# Start the while loop
while [ $counter -le 3 ]
do
    echo "Counter: $counter" 
    # Increment the counter
    ((counter++))
done
```
- `counter=1` creates a variable named counter is initialized to 1. This variable will control the number of iterations in the loop.

- `while [ $counter -le 3 ]`: The while loop starts with the condition `[ $counter -le 3 ]`. The loop will continue to execute as long as the value of counter is less than or equal to `(-le)` 3.

- `do` marks the start of the block of commands to run for each iteration

- `echo "Counter: $counter"`: Inside the loop, the value of the counter variable is printed

- `((counter++))`: This command increments the counter variable by 1.

- `done`: This marks the end of the `while` loop. The loop then checks the condition `[ $counter -le 3 ]` again. If it is true, the loop executes again; otherwise, it stops.

The output of the script is:

```bash
Counter: 1
Counter: 2
Counter: 3
```
It is important to properly increment the `counter` variable so the condition eventually becomes false. If the `counter` variable is not updated appropriately, the `while` loop will run indefinitely.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Using For Loops

The `for` loop in shell scripting allows you to iterate until a condition is met. It’s commonly used to automate repetitive tasks by executing a block of code multiple times. The syntax of a `for` loop is:

```bash
#!/bin/bash
for ((initialization; condition; update))
do
  commands
done
```
- `initialization`: This sets the starting value for the loop control variable. It is executed only once at the beginning of the loop.

- `condition`: This is a boolean expression. The loop continues to execute as long as this condition is true. It is evaluated before each iteration.

- `update`: This operation updates the loop control variable after each iteration of the loop.

- `do`: This keyword indicates the beginning of the block of commands that will be executed in each iteration of the loop.

- `commands`: These are the commands or code that will be executed each time the loop iterates.

- `done`: This keyword marks the end of the loop block. After executing the commands, control goes back to the `update` step and the condition is checked again. If the condition is true, the loop runs again; otherwise, it terminates.

Here’s a simple example to illustrate the syntax:

```bash
#!/bin/bash
for ((x=1; x<=3; x++))
do
  echo "Iteration $x"
done
```
- The loop control variable `x` is initialized to 1, and the loop continues as long as `x` is less than or equal to 3. The variable `x` is incremented by 1 after each iteration.

- `echo "Iteration $x"`: This command prints the current value of `x` during each iteration.

- `done`: This marks the end of the loop. The loop will repeat until the condition `x<=3` is no longer true.

The output of the script is:

```bash
Iteration 1
Iteration 2
Iteration 3
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Using "for in" loops

The `for in` loop in shell scripting allows you to iterate over a sequence of values or elements. The syntax of a `for` loop is:

```bash
#!/bin/bash
for variable in list
do
    command1
    command2
    ...
done
```
- `variable` is a loop control variable that takes the value of each element in the list one by one.

- `list` can be a sequence of numbers, strings, or an array.

- The `do...done` block contains the commands to be executed in each iteration.

Let's see a concrete example looping over a range of numbers. A range is defined as `{start..end}`. Unlike some coding languages,`end` is **inclusive**. Let's take a look:

```bash
#!/bin/bash
# Script to demonstrate for loop
for i in {1..5}
do
  echo "Iteration $i"
done
```
- `for i in {1..5}`: The loop control variable `i` takes values from 1 to 5.

- `do ... done`: This specifies the block of code to run for each iteration

- `echo "Iteration $i"`: Prints the current iteration number.

The output of the above script is:

```bash
Iteration 1
Iteration 2
Iteration 3
Iteration 4
Iteration 5
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Looping Through Arrays

Combining arrays and loops, you can iterate over array elements by combining for in with `"${array_name[@]}"`. Here's an example

```bash
#!/bin/bash
# Looping through array
computers=("Dell" "HP" "Lenovo")

for computer in "${computers[@]}"
do
    echo "$computer"
done
```
- `for computer in "${computers[@]}"`: The `for` loop starts with the control variable `computer`.

- `do ... done`: The code inside this block is run during each iteration.

- `echo "$computer"`: The loop prints the current value of the `$computer` variable from the `computers` array

The output of the script is:

```bash
Dell
HP
Lenovo
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

## Functions in Shell Scripts

Welcome to this lesson about **functions in shell scripts**, a powerful concept that helps you organize your code efficiently. Functions allow you to group commands into reusable blocks, making your scripts cleaner and easier to manage. By the end of this lesson, you'll have a solid grasp of creating and using functions in your shell scripts.

Let's dive in!

&nbsp;&nbsp;&nbsp;

### Introduction to Functions

Functions in shell scripts are similar to those in other programming languages. They help you encapsulate code, making it modular, reusable, and easier to debug. Think of a function as a mini-script within your main script that you can call whenever needed.

&nbsp;&nbsp;&nbsp;

### Defining and Calling Functions

The basic syntax for defining a function in shell scripts is straightforward. Here’s how you can define and call a simple function:

```bash
#!/bin/bash
# Defining a function
function_name() {
  # Commands to be executed
}

# Calling the function
function_name
```
Let's start by defining a simple function:
```bash
#!/bin/bash
# Defining a function
greet() {
  echo "Hello"
}

greet
```
- `greet() { ... }`: This block defines a function named `greet`.

- `echo "Hello"`: This command inside the function prints "Hello"

- `greet`: This line calls the function `greet` resulting in the output "Hello".

&nbsp;&nbsp;&nbsp;

### Passing Arguments to Functions

In shell scripting, you cannot directly pass named parameters to functions in the same way as you might in some other programming languages. Instead, you pass positional parameters. When calling a function, each parameter is assigned a position number, starting at 1. These arguments are then accessed inside the function using `$position_number`. Let's take a look:

```bash
#!/bin/bash
# Function with 1 input
update_one() {
  echo "Updating $1"
}

# Function with 2 inputs
update_two() {
    echo "Updating $1 and $2"
}

update_one "Photos"
update_two "Photos" "Browser"

```
- `update_one` is a function that takes one input parameter (`$1`). The function prints the string "Updating" followed by the value of the input parameter.

- `update_two` is a function that takes two input parameters (`$1` and `$2`). The function prints the string "Updating" followed by the values of the two input parameters, separated by "and".

- `update_one "Photos"` calls the `update_one` function and the positional argument `$1` is "Photos"

- `update_two "Photos" "Browser"` calls the update_two function. The positional argument `$1` is "Photos" and the positional argument `$2` is "Browser".

The output of this script is:

```
Updating Photos
Updating Photos and Browser
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Counting Arguments with '$#'

In addition to handling specific arguments, you may want to know the total number of arguments passed to a function. You can do this using the special variable `$#`, which holds the number of positional parameters.

```bash
#!/bin/bash
# Function to count arguments
count_args() {
    echo "Number of arguments: $#"
}

count_args "Photos" "Browser" "Documents"
```
- `count_args() { ... }`: This block defines a function named `count_args`.

- `echo "Number of arguments: $#"`: This command prints the total number of arguments passed to the function.

- `count_args "Photos" "Browser" "Documents"` calls the `count_args` function with three arguments, so the output will be `Number of arguments: 3`

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Iterating Over Arguments Using Arrays and '$@'

In shell scripting, you might find yourself needing to iterate over arguments both as an array and using the `$@` special parameter. You can convert the arguments to an array using `args=("$@")`. You can iterate over the arguments directly using `"$@"`.

Below is an example that demonstrates both methods within a function:

```bash
#!/bin/bash
print_args() {
  arr=("$@")
  
  echo "Using array indices:"
  for x in "${arr[@]}"
   do
    echo "$x"
  done
  
  echo "Using \$@:"
  for y in "$@"
  do
    echo "$y"
  done
}

print_args "Photos" "Browser" "Documents"

```
- `arr=("$@")`: This command assigns all positional parameters to an array named `arr`.

- `for x in "${arr[@]}"; do`: This loop iterates over each element in the arr array, printing each argument.

- `for y in "$@"; do`: This loop iterates directly over the positional parameters using `$@`.

When calling `print_args "Photos" "Browser" "Documents"`, the output will be:

```Plain text
Using array indices:
Photos
Browser
Documents
Using $@:
Photos
Browser
Documents
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Returning Values from Functions

While functions can echo or print output directly, you can also return values using echo and capture them using command substitution. For example:
```bash
#!/bin/bash
# Function to increase a version number
increase_version() {
  ((version += increment))
  echo $version
}

version=2
increment=3
# Capture the return value using command substitution
result=$(increase_version)          
echo "Updating from version $version to $result"   # Prints: "Updating from version 2 to 5"
```
- `increase_version() { ... }`: This block defines a function named `increase_version`.

- `((version += increment))`: This command increments the `version` number by the value of `increment`.

- `echo $version`: This command prints the updated `version` number.

- `result=$(increase_version)`: This line captures the function’s output using command substitution.

- `echo "Updating from version $version to $result"`: This command prints the message with the original and updated version numbers.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

## File Operations

Introduction to File Operations in Bash
Welcome! In this lesson, we will dive into essential file operations using Bash. File operations are tasks performed on files, such as creating, reading, writing, copying, and deleting them. These operations are fundamental for managing data and organizing information on a computer system. Shell scripting with Bash (Bourne Again SHell) provides a powerful and flexible way to automate and streamline file operations. By using simple commands, you can perform complex tasks efficiently, saving time and reducing the potential for human error. Bash scripting is especially useful for repetitive tasks, large-scale file management, and system administration.

&nbsp;&nbsp;&nbsp;

### Creating a File

Let's start by creating a new file.

The touch command creates an empty file if the given file name doesn't already exist. If the file does already exist, touch updates the file's last modified timestamp without changing its content. Let's create a file called `example.txt`.

```bash
#!/bin/bash
touch example.txt
```
The command creates a new file called `example.txt` in the current working directory (We will cover directories in more detail later in this course). If `example.txt` already exists, this command updates its last modified timestamp.

`example.txt` can now be opened and edited. In the practice exercises, you can open this file by clicking the `☰` icon to the right of Cosmo's message.


&nbsp;&nbsp;&nbsp;










## Echo Text Control in Bash

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Introduction to Echo Text Control in Bash

Welcome to your first lesson on text processing with Bash! In this lesson, we'll dive deep into the echo command. As you know, the echo command in Bash allows you to print text to the terminal. Though it may seem simple, the echo command is powerful and versatile, and mastering it is essential for formatting messages, debugging scripts, and more. Now, let's dive into the advanced text manipulation with echo.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

```bash
#!/bin/bash
# Default new line
echo "Greetings"
echo "Universe"

# Suppress new line
echo -n "Hello"  # Prints "Hello" without newline
echo "World"    # Prints "World" on the same line as "Hello"

```bash
output:
Greetings
Universe

HelloWorld
```
Output:
```bash
Greetings
Universe

HelloWorld
```
The first two echo commands by default add a new line at the end of the output. This results in "Greetings" and "Universe" being printed on new lines. By including `-n` before "Hello", the new line is suppressed resulting in "Hello" and "World" being printed on the same line.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Escape Characters

Escape Characters
Escape characters are special sequences in a string that denote a particular character that cannot be easily typed or is otherwise reserved. In Bash, escape characters are introduced with a backslash (). They allow you to include characters in your output that have special meanings or are not readily available on the keyboard. To include escape characters in a string, you use the syntax:
```bash
echo -e "..."
```
When you use the `-e` option with the echo command, these escape sequences are interpreted and displayed as their corresponding special characters.

Some common escape characters are:

- `\n` for a new line

- `\t` for a tab

- `\\\` for a backslash

- `\"` for a quote

Let's look at some examples!

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Adding New Lines

`\n` is an escape character that represents a newline in a string. When used with the `echo -e`, it instructs the terminal to move the cursor to the beginning of the next line, effectively splitting the text at that point into multiple lines. Here's an example:

```bash
#!/bin/bash
echo -e "This is line 1.\nThis is line 2." 
```
Output:
```bash
This is line 1.
This is line 2.
```
Using `\n`, you can now print multiple lines with a single `echo` command.

In contrast, omitting `-e` in the above statement would display:

```bash
This is line 1.\nThis is line 2.
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Adding Tab Spaces

`\t` is an escape character that represents a horizontal tab in a string. When used with `echo -e`, it inserts a tab space at the specified position in the text, creating a gap between text segments. In Bash, `\t` is typically 4 to 8 spaces but it is configurable. Here's an example:
```bash
#!/bin/bash
echo -e "Column1 \tColumn2 \tColumn3"
```

The output is:
```bash
Column1    Column2    Column3
```
The `\t` special character is great for formatting output into columns or aligning text.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Printing Double Quotes

Double quotes are used to define string literals. When you open a string with a double quote (`"`), Bash expects the string to be closed by the next double quote. For example, `"Name: "Cosmo" "` results in an error because Bash interprets the quote before C as the closing quote of the string. To include double quotes in a string, you add a `\` beforehand. Let's look at an example:
```bash
#!/bin/bash
echo -e "Welcome to this lesson titled \"Text Formatting with Echo\""
```
Including quotes in your strings is essential for properly formatting quoted phrases or titles.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Printing Backslashes

Windows-style file paths use backslashes (`\`) as separators between directory levels, which is different from Unix-like systems that use forward slashes (`/`). You might be wondering how to include a `\` in such a string. Bash expects the text after `\` to be an escape character that has special meaning like `n` or `t`. Let's take a look at an example.

```bash
echo -e "C:\temp"
```

The output is:

```bash
C:      emp
```
Bash interprets the `t` in `temp` as the tab character. Just like n and `t`, `\` is also an escape character. To include a `\` in a string, you just use `\\\`. For example:

```bash
#!/bin/bash
echo -e "C:\\\Users\\\Cosmo\\\report.txt"
```

The output is:
```bash
C:\Users\Cosmo\report.txt
```
Using `\\\` is essential for correctly displaying file paths or other text containing backslashes in your output.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Displaying Colored Text

The `echo` command can also be used to display colored text using ANSI escape codes. The syntax to change the color of text is:

```bash
echo -e "\e[<ANSI_code>m<Your text>\e[0m"
```

- `\e`: This initiates the ANSI escape sequence.

- `[<ANSI_code>m`: This is the ANSI escape code for setting text formatting attributes, such as color. The <ANSI_code> is a specific number that represents a particular color.

- `<Your text>` is the actual text you want to print in the specified color.

- `\e[0m`: This sequence resets the text attributes (including color) back to their default settings. This ensures that any text printed after this sequence appears in the default color and formatting.

Let's take a look at an example:

```bash
#!/bin/bash
echo -e "\e[31mThis text is red.\e[0m"  # Prints red text
echo -e "\e[32mThis text is green.\e[0m"  # Prints green text
```
- `\e[31m` starts red-colored text.

- `\e[32m` starts green-colored text.

- `\e[0m` resets the color back to the default.

The output is:
```
This text is red.
This text is green.
```
&nbsp;&nbsp;&nbsp;

Here is a list of some common color codes:

- `30`: Black

- `31`: Red

- `32`: Green

- `33`: Yellow

- `34`: Blue

- `35`: Magenta

- `36`: Cyan

- `37`: White

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

## File Name Expansion and Management in Bash

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Introduction to File Name Expansion and Management

Welcome! In this lesson, we will focus on file name expansion and management in **Bash**. File name expansion allows you to match file names using patterns. For example, you might want to find all `.txt` files in a directory or list all files that start with the word `data`. This lesson serves as a foundational block for managing files, significantly easing the manipulation and organization of files in more complex scripts. Let's get started!

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Wildcards and Basic Expansion

File name expansion, also known as globbing, is a feature in shell environments like Bash that allows you to match filenames using patterns with special characters called wildcards. These wildcards are symbols like `*`, `?`, and `[]`. These wildcards allow you to easily select files that fit certain criteria without typing each filename explicitly. This simplifies file management tasks such as listing, copying, or moving multiple files at once.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Matching Any Number of Characters: '*'

The `*` wildcard matches any number of characters (including none). This is useful when you need to list or act on files with similar names but different suffixes or prefixes.

For example, `ls *.txt` lists all files ending with `.txt`. The `*` wildcard before `.txt` can represent any sequence of characters (including none), so any file name of any length that ends in `.txt` will be listed. Let's take a look:
```bash
#!/bin/bash

# Create example files
touch file1.txt draft.txt file2.sh

ls *.txt
```
Output:
```
draft.txt
file1.txt
```
Only the files that end with `.txt` are included. `file2.sh` is not included because it does not match the specified pattern.

---

Now let's use `*` to search for any files that begin with file. ls `file*` matches any name that begins with `file` followed by any sequence of characters.

```bash
#!/bin/bash

# Create example files
touch file1.txt draft.txt file2.sh

ls file*
```

Output:
```bash
file1.txt
file2.sh
```

Only the files that start with `file` are included. `draft.txt` does not match the pattern, so it is not included.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Matching Exact Character Counts: '?'

The `?` wildcard matches exactly one character. This is useful for filtering files that have specific character lengths.

For example, `ls draft?.md` matches any file that starts with `draft` and has one character before `.md`

```bash
#!/bin/bash

# Create example files
touch draft1.md draftA.md draft1A.md

ls draft?.md
```
Output:
```
draft1.md
draftA.md
```

`draft1A.md` is not included in the output because it has two characters between `draft` and `.md`.

---

You can match multiple characters by using any number of `?` wildcards. Let's examine ls `draft??.md`. Two `?` wildcards represent exactly two characters, so any file name that starts with `draft` and has two characters before `.md` will be listed.

```bash
#!/bin/bash

# Create example files
touch draft1.md draftA.md draft1A.md

ls draft??.md
```
Output:
```
draft1A.md
```
`draft1.md` and `draftA.md` are not included because they only have a single character between `draft` and `.md`

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Matching Ranges: '[]'

The `[]` wildcard allows you to match a specific set or range of characters. You can specify ranges of characters by using a hyphen (-) between two characters. A range represents all characters between the two specified characters, inclusive.

Let's look at `ls file[1-3].txt`. The `[1-3]` wildcard will match any one character from 1 to 3.

```bash
#!/bin/bash

# Create example files
touch file.txt file1.txt file2.txt file4.txt

ls file[1-3].txt
```
Output:
```
file1.txt
file2.txt
```
The command does not match `file.txt` because there is no digit between `file` and `.txt`. `file4.txt` is not matched because `4` does not fall in the `[1-3]` range.

---

To include multiple ranges in the `[]` wildcard, you simply list them side by side within the brackets. file`[1-35-6]`.txt will match any name that starts with file followed by the numbers `1`, `2`, `3`, `5`,`6`, then ends with `.txt`

Let's take a look:

```bash
#!/bin/bash

# Create example files
touch file1.txt file2.txt file3.txt file4.txt file5.txt file6.txt file7.txt

ls file[1-35-6].txt
```

Output:
```
file1.txt
file2.txt
file3.txt
file5.txt
file6.txt
```
`file4.txt` and `file7.txt` are not included because they fall outside of the ranges `1-3` and `5-6`

---

Similarly, you can use letters in ranges as well.

```bash
#!/bin/bash

# Create example files
touch fileA.txt fileB.txt fileC.txt fileD.txt 

ls file[A-C].txt
```

Output:
```
fileA.txt
fileB.txt
fileC.txt
```
The `[A-C]` range matches A, B, and C, so `fileA.txt`, `fileB.txt`, and `fileC.txt` are listed. `fileD.txt` is not listed because `D` is outside the [A-C] range.

---

If you want to include a range that is only a single character, you use just that character without a hyphen.

```bash
#!/bin/bash

# Create example files
touch file1.txt file2.txt file4.txt
touch fileA.txt fileB.txt fileC.txt

ls file[1-3B].txt
```
Output:
```
file1.txt
file2.txt
fileB.txt
```

This pattern matches any name that start with `file`, followed by a `1`, `2`, `3`, or `B`, then ends with `.txt`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Matching Lists: '{}'

The `{}` expansion is used to generate arbitrary strings. Think of this as an **OR** clause where each pattern inside the braces is treated as an alternative and expands into multiple words. This is useful when you want to match specific filenames that follow different patterns.

Let's look at `file{1,2,1A}.txt`. This will match the names `file1.txt`, `file2.txt`, and `file1A.txt`.

```bash
#!/bin/bash

# Create example files
touch file1.txt file2.txt file1A.txt file2A.txt

ls file{1,2,1A}.txt
```
Output:
```bash
file1A.txt
file1.txt
file2.txt
```
`file2A.txt` does not get matched because `2A` is not one of the specified patterns listed in `{}`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Combining Wildcards for Flexible Matching

You can combine all these wildcards into a single query for advanced pattern matching. Let's inspect this pattern

```bash
{file,draft}[1-3A]??.*
```
- `{file,draft}` matches either file or draft.

- `[1-3A]` matches any one character from the set `1`, `2`, `3`, or `A`.

- `??` matches exactly two characters, regardless of their values.

- `. *` matches a dot followed by any sequence of characters, representing any file extension.

```bash
#!/bin/bash

# Files that will match
touch file123.txt draft1AA.pdf fileAAA.sh
# Files that will not match
touch script123.txt file423.txt draft12.txt draft1234.txt file123_pdf

ls {file,draft}[1-3A]??.*
```
Output:

```
draft1AA.pdf
file123.txt
fileAAA.sh
```
Combining these wildcards allows you to perform complex file name matches effortlessly, making your file management tasks in Bash more powerful and efficient.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Case Sensitivity

Lastly, let's cover case sensitivity. File name expansion is case-sensitive by default. This means that wildcards will distinguish between uppercase and lowercase letters. For example, `*` will treat `file.txt` and `File.txt` as different files.

If you want a case-insensitive match, you can use a case-insensitive shell option called `shopt -s nocaseglob`. This makes wildcards ignore case sensitivity. To revert back to case-sensitive matching, you can disable this option using `shopt -u` `nocaseglob`:

```bash
#!/bin/bash

# Enable case-insensitive globbing
shopt -s nocaseglob

# Create example files
touch file.txt File.md

ls file.*

# Disable case-insensitive globbing
shopt -u nocaseglob
```
Output:

```
File.md
file.txt
```
With nocaseglob enabled, both `file.txt` and `File.md` are matched.
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

## Pattern Matching and Search with Grep

### Introduction to Pattern Matching and Search with Grep

Welcome! In this lesson, we will explore the powerful tool `grep` in Bash for pattern matching and searching within files. `grep` stands for "Global Regular Expression Print," and it is widely used for filtering and searching through text data. Think of `grep` like automating `Ctrl+F` within your scripts. Mastering `grep` will make your text processing tasks easier and more efficient. Let's get started!

&nbsp;&nbsp;&nbsp;

### Basic Search

Let's start with a basic search using grep. The basic command syntax is:

```bash
grep 'pattern' filename
```
This command searches for the specified pattern within the file. Here's an example:
```bash
#!/bin/bash
echo -e "This line will match\nfile.txt is a test file\nHello World" > file.txt

grep 'is' file.txt
```
Output:
```
This line will match
file.txt is a test file
```
This query outputs only the lines that contain `is` anywhere in the line. Notice that "This line will match" is included because "This" contains the substring "is".

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Search for Whole Words Only

To search for a whole word rather than a substring, use the -w option. Let's take a look:

```bash
#!/bin/bash

echo -e "This line won't match\nfile.txt is a test file" > file.txt
grep -w 'is' file.txt
```
Output:
```
file.txt is a test file
```
Even though "This" contains the substring "is", only the lines with "is" as a whole word are matched.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Case-Insensitive Search

Sometimes, you need to perform a search without considering the case (uppercase or lowercase). The `-i` option allows for case-insensitive searches. For example:
```bash
#!/bin/bash
echo -e "Hello World\nHELLO WORLD\nHi world!" > file.txt
grep -i 'hello' file.txt
```

Output:
```
Hello World
HELLO WORLD
```
In this example, "Hello" and "HELLO" are both matched because `-i` ignores case differences.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Search for Lines Beginning with a Pattern

To find lines that begin with a specific pattern, you can use the caret (^) symbol. For example, ^Hello will match any lines that start with Hello. This anchor signifies the start of a line in regular expressions. Let's look at an example:

```bash
#!/bin/bash

echo -e "Hello world\nSay Hello grep\nGreetings" > file.txt
grep '^Hello' file.txt
```

Output:
```
Hello world
```
Only the lines that begin with Hello are matched. The caret `^` ensures that the pattern must appear at the start of the line.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Show Line Numbers

If you want to know the line numbers of matching patterns, you can use the `-n` option. Let's look at an example:

```bash
#!/bin/bash
echo -e "Hello world\nGrep is a powerful tool\nThis is a test file\nHello grep" > file.txt

grep -n 'grep' file.txt
```

Output:
```
2:Grep is a powerful tool
4:Hello grep
```
Both lines 2 and 4 contain "Grep". Notice that line numbers start at 1, not 0

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Invert Match

To find lines that do not match a given pattern, use the `-v` option. Suppose we have the following script:

```bash
#!/bin/bash

echo -e "Hello world\nGrep is a powerful tool\nThis is a test file\nHello grep" > file.txt
grep -v 'Hello' file.txt
```

Output:
```
Grep is a powerful tool
This is a test file
```
Using `-v`, only lines that do not contain "Hello" are printed

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Count Matches

To count the number of lines matching a pattern, use the `-c` option. For example:

```bash
#!/bin/bash

echo -e "This file is a test file\nSearch in a file using grep\nThis won't match" > file.txt
grep -c 'file' file.txt
```

Output:

```
2
```
Only 2 of the 3 lines contain the word "file". `-c` only counts matching lines. "This file is a test file" contains "file" twice, but the line is only counted once.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Search for Multiple Patterns (OR Logic)

You can simulate `OR` logic using the `-e` option. This allows you to search for lines 

```bash
#!/bin/bash

echo -e "Introducing grep\nHello world\nThis won't match\nHello grep" > file.txt
grep -e 'Hello' -e 'grep' file.txt
```

Output:

```
Introducing grep
Hello world
Hello grep
```
`grep -i 'grep' file.txt` matches the 3 lines that contain grep (case-insensitive). These 3 lines are then sent as input to `grep -i 'hello' `which matches only the lines that contain hello (case-insensitive.)

---

Perform basic searches using grep:

Use `-w` to search for whole words

Use `-i` to conduct case-insensitive searches

Use `-n` to show line numbers of matching patterns

Use `-v` to invert matches

Use `-c` to count the number of matching lines

Use `-e` to implement OR logic to search for multiple patterns

Use `|` to implement AND logic by piping multiple commands together

---

Example :
```bash
#!/bin/bash

echo -e "Lines containing \"successful\":"
# TODO: Modify the query to search for whole words
grep -w 'successful'  system.logs
echo

# TODO: Modify the command to search for lines that start with ERROR
echo -e "Lines containing \"ERROR\":"
grep '^ERROR' system.logs
echo

# TODO: Modify the command to search for lines that do not start with "INFO"
echo -e "Lines not starting with \"INFO\":"
grep -v '^INFO' system.logs
echo

# TODO: Modify the command to search for "disk" or "memory" and ignoring case
echo -e "Lines containing \"disk\" or \"memory\":"
grep -i -e 'disk' -e 'memory' system.logs
echo
```

---

Example 2
```bash
#!/bin/bash

echo "All users with Admin privileges are required to renew their credentials."
# TODO: Find all users with Privilege Level "Admin"
grep -w 'Admin' users.txt
echo -e "\nAll users with a software version starting with \"1.\" need to update their software."
# TODO: Find users with a software version starting with "1."
grep -E '1\.[0-9]+' users.txt
# Hint: You need to escape the period

echo -e "\nAll users who are not SuperAdmin must review the new security protocols."
# TODO: Find all users without "SuperAdmin" privileges
grep -v 'SuperAdmin' users.txt

echo -e "\nAll users with User privileges in Engineering should attend the Admin onboarding session."
# TODO: Find all users with "User" privileges in the Engineering department.

grep 'User' users.txt | grep 'Engineering'

```
---

```bash
#!/bin/bash

# Clear all files
> report1.txt
> report2.txt
> report3.txt
> summary.txt
> errors.log

echo "Server 1 Report" >> report1.txt
# TODO: Append all lines that contain "Server 1" (include line numbers) to report1.txt
grep -n 'Server 1' system.log >> report1.txt
echo "Server 2 Report" >> report2.txt
# TODO: Append all lines that contain "Server 2" (include line numbers) to report2.txt
grep -n 'Server 2' system.log >> report2.txt
echo "Server 3 Report" >> report3.txt
# TODO: Append all lines that contain "Server 3" (include line numbers) to report3.txt
grep -n 'Server 3' system.log >> report3.txt
echo "Connection ERROR Entries" >> errors.log
# TODO: Append all lines containing "ERROR" AND "Connection" (case-insensitive) to errors.log
grep -i 'ERROR' system.log | grep 'Connection' >> errors.log
echo "Memory ERROR Entries" >> errors.log
# TODO: Append all lines containing "ERROR" AND "Memory" (case-insensitive) to errors.log
grep -i 'ERROR' system.log | grep 'Memory' >> errors.log
echo "CPU ERROR Entries" >> errors.log
# TODO: Append all lines containing "ERROR" AND "CPU" to errors.log
grep 'ERROR' system.log | grep 'CPU' >> errors.log
echo -n "Number of 'WARNING' Entries: " >> summary.txt
# TODO: Count the number of lines that begin with WARNING and append it to summary.txt
grep -c '^WARNING' system.log >> summary.txt
echo -e "\nIP Addresses" >> summary.txt
# TODO: Append all lines containing IP (whole word) to summary.txt
grep -w 'IP' system.log >> summary.txt
echo -e "\nHigh and Low Level Monitoring:" >> summary.txt
# TODO: Append all lines with "High" or "Low" (case insensitive) to summary.txt
grep -i -E 'High|Low' system.log >> summary.txt
```
---

Example 3
```bash
#!/bin/bash

echo -e "Lines containing \"successful\":"
# TODO: Modify the query to search for whole words
grep -w 'successful'  system.logs
echo

# TODO: Modify the command to search for lines that start with ERROR
echo -e "Lines containing \"ERROR\":"
grep '^ERROR' system.logs
echo

# TODO: Modify the command to search for lines that do not start with "INFO"
echo -e "Lines not starting with \"INFO\":"
grep -v '^INFO' system.logs
echo

# TODO: Modify the command to search for "disk" or "memory" and ignoring case
echo -e "Lines containing \"disk\" or \"memory\":"
grep -i -e 'disk' -e 'memory' system.logs
echo
```
---

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

## Text Substitution and Editing with Sed

### Introduction to Text Substitution and Editing with Sed

Welcome! In this lesson, we will explore the powerful text processing tool `sed` in Bash. `sed` stands for "Stream Editor," and it is a versatile utility for parsing and transforming text in files or data streams. It reads text from a file or standard input, processes it according to specified commands, and outputs the modified text. It's commonly used for tasks like text substitution, deletion, and insertion in both files and data streams, making it an essential tool for automating text processing in scripts.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Understanding Text Substitution with Sed

Text substitution is one of the fundamental uses of `sed`. Suppose you want to replace all instances of the word "pattern" with "replacement" in a file. The syntax for this:

```bash
sed 's/pattern/replacement/' filename
```
This command searches for the **first occurrence** of the specified pattern and replaces it with `replacement`. The new text will then be output to the terminal. Let's replace the first occurrence of "Hi" with "Hello":

```bash
#!/bin/bash

echo "Hi World. Hi sed." > file.txt
echo "Output of sed command:"
sed 's/Hi/Hello/' file.txt

echo -e "\nContents of file.txt:"
cat file.txt
```

Output:

```
Output of sed command:
Hello World. Hi sed.

Contents of file.txt:
Hi World. Hi sed.
```
&nbsp;&nbsp;&nbsp;

This command searches `file.txt` for the first occurrence of "Hi" and replaces it with "Hello" and outputs it to the terminal. "Hi sed." does not change to "Hello sed" because this command only searches for the **first** occurence of "Hi". Also notice that the contents of `file.txt` were not actually changed.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### In-place Substitution

In many cases, you may want to make substitutions directly within the file. The `-i` option allows you to do this. You must ensure that the file has write permissions so `sed` can write to it. For this, you use the` chmod +w` command.

```bash
#!/bin/bash

echo "Hi World" > file.txt
chmod +w file.txt
sed -i 's/Hi/Hello/' file.txt

cat file.txt
```

Output:
```
Hello World
```

Using `-i`, the contents of `file.txt` have been replaced. Notice that the actual sed command does not print anything to the terminal now.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Global Substitution

To replace all occurrences of a pattern in the file, add the `g` flag to the `sed` command.

```bash
#!/bin/bash

echo "Hi World. Hi sed." > file.txt
sed 's/Hi/Hello/g' file.txt
```

Output:
```
Hello World. Hello sed.
```
Now, all instances of `Hi` are replaced with `Hello`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Deleting Lines with Sed

`sed` can also be used to delete specific lines from a file. The syntax for deleting lines is:

```bash
sed 'pattern/d' filename
```
Let's look at an example:

```
#!/bin/bash

echo -e "Hello World\nDelete me" > file.txt
sed '/Delete/d' file.txt
```
Output:
```
Hello World
```
The line `Delete me` has been deleted.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Inserting Lines with Sed

`sed` can be used to insert lines after a specific pattern. The syntax for insertion is:

Let's add "Nice to meet you" after any line that contains the text "Hello"

```bash
#!/bin/bash

echo -e "Hello World\nThis won't match\nHello sed" > file.txt
sed '/Hello/a Nice to meet you.' file.txt
```
Output:

```
Hello World
Nice to meet you.
This won't match
Hello sed
Nice to meet you.
```
Any line that contains `Hello` now has the line `Nice to meet you`. after it.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Saving Changes to a New File

Sometimes, you might want to save the edited content to a new file instead of modifying the original. Suppose we want to create a new file `grep.txt` that contains the contents of `sed.txt` with all instances of `sed` changed to `grep`. To do this, we create the new `grep.txt` file and grant write permissions. Then we simply redirect the output of the `sed` command using `>`. Let's take a look:

```bash
#!/bin/bash

echo -e "sed allows for efficient text management.\nWriting a script using sed automates tedious manual tasks." > sed.txt

# Create grep.txt and allow write permissions
touch grep.txt
chmod +w grep.txt

# Save to new file
sed 's/sed/grep/g' sed.txt > grep.txt

cat grep.txt
```
Output:
```
grep allows for efficient text management.
Writing a script using grep automates tedious manual tasks.
```
Now, `sed.txt` still contains its original content, and the new `grep.txt` file contains the results of the `sed` command.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Advanced Pattern Matching

In addition to the basic commands we've covered, `sed` supports advanced pattern matching using wildcards and regular expressions for more flexible and powerful text processing. Let's explore how to leverage some of these features:

---
`.` matches any single character except a newline. Suppose we want to ensure the punctuation after "World" is always an "!". The pattern we want to match is `/World./`. This will match any instance of "World" followed by any single character. Let's see an example:

```bash
#!/bin/bash
echo "Hello World? Hello World%" | sed 's/World./World!/g' 
```
Output:
```
Hello World! Hello World!
```
The pattern matches `World?` and `World%`. These are replaced with `World!`

---

`*` matches zero or more occurrences of the preceding character. Suppose we want to reduce multiple spaces between words to a single space. The pattern we want to match is`s/` `*/`. This pattern contains two spaces. The `*` will match zero or more occurrences of the preceding character which is a single space. This matches any sequences of two or more spaces. We will then replace it with `/ /` which is a single space. Let's see this in action:

```bash
#!/bin/bash
echo "Hello    World!   This  is  a   test." | sed 's/  */ /g'
```

Output:

```
Hello World! This is a test.
```

This finds any sequence of two or more spaces and replaces them with a single space.

---

`[]`: Matches any one of the enclosed characters. Suppose we want to replace all digits in a sentence with the character `#`. The pattern to search for is` /[0-9]/`, which matches any digit. We will then replace it with `/#/`. Here's an example:

```bash
#!/bin/bash
echo "My phone number is 123-456-7890." | sed 's/[0-9]/#/g'
```
Output:
```
My phone number is ###-###-####.
```
Now, all digits have been replaced with `#`.

---

`\b` matches a word boundary, which is the position between a word and a space or punctuation. Suppose we want to replace the word sed with grep. The pattern to match is \bsed\b, which ensures that only the standalone word sed is matched. We then replace it with grep. Let's take a look:

```bash
#!/bin/bash
echo "sed is used for text processing." | sed 's/\bsed\b/grep/'
```
Output:
```
grep is used for text processing.
```
Notice that even though `used` contains `sed`, it does not get replaced with `grep`. This is because there is no word boundary before `sed` in the string `used`.

1. Perform text substitution using sed `'s/pattern/replacement/'`

2. Use `-i` to make in-place changes directly in the file

3. Add the `g` flag to perform global substitution

4. Use sed 'pattern/d' to delete specific lines from a file with

5. Use `sed 'pattern/a\new_line'` to insert lines after a specific pattern

6. Use the `.` wildcard to match any single character

7. Use the `*` wildcard to match zero or more occurrences of the preceding character

8. Use the `[]` character class to match specific characters or ranges of characters

9. Use the `\b` word boundary to precisely match whole words

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


## Text Processing with awk

### Introduction to Text Processing with 'awk'

Hello! Welcome to your next step in mastering Bash scripting. In this lesson, we will immerse ourselves in the world of text processing with the versatile command-line tool `awk`. `awk` is a powerful tool that allows you to manipulate and analyze text files with ease. By the end of this lesson, you’ll be equipped to efficiently handle and process text files, extracting meaningful data and performing relevant computations directly from your Bash scripts.

Let's get started by diving into how we can leverage `awk` for various text processing tasks.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Creating Initial Data

First, let's create a sample data file to work with. This file will help us learn and practice various `awk` commands effectively.

The heredoc (short for "here document") is a special syntax in Unix shell scripting that allows you to create a multi-line string. It is particularly useful for creating files or including large blocks of text within your script. The syntax is `<<EOF ... EOF`, where `EOF` (End of File) is a marker indicating the beginning and end of the block of text. You can actually use any marker, but EOF is conventionally used.

Let's create a file called `data.txt` that includes data about computers in inventory.

```bash
#!/bin/bash

# Create a sample data file
cat << EOF > data.txt
Brand   Model     RAM
Apple   MacBook    32
Apple   iPad       16
Dell    XPS        32
Dell    Inspiron  128
Lenovo  ThinkPad  128
Lenovo  Yoga      256
Apple   MacBook    64
EOF
```

Let's break this code down:

- `cat << EOF`: This starts the heredoc and tells the cat command to begin reading the subsequent lines as a string until it encounters the ending `EOF` marker.

- `> data.txt`: This redirects the output of the `cat` command to a file named `data.txt`.

- The lines between `<< EOF` and `EOF` are the content that will be written to `data.txt`.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Basic Syntax of 'awk'

The basic syntax of the awk command in Unix-like systems is:

```bash
awk options 'selection_criteria {action}' input-file > output-file
```

Here's a detailed breakdown of each component:

- `awk`: The command itself.

- `options`: These are optional flags you can pass to `awk` to modify its behavior (e.g., `-F` to specify the field separator).

- `selection_criteria`: This is an optional condition or pattern that specifies which lines of the input file to process. It can be a regular expression or a logical condition based on field values.

- `{action}`: This is the block of code to execute for each line that matches the selection criteria. Actions are enclosed in curly braces `{}`.

- `input-file`: The file that awk processes.


- `> output-file`: This optional part redirects the output to a file. If omitted, awk prints the output to the terminal..

With this understanding of `awk` syntax, let's dive into some examples.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Printing Entire File Using 'awk'

Let's begin with the most basic `awk` command to print the entire content of the file. The `print` command is used to output text, fields, or expressions to the terminal or another output stream. It offers flexibility in how the data is displayed and allows custom formatting of text.

```bash
#!/bin/bash

# Using `awk` to print the entire file
awk '{print}' data.txt
```

In this `awk` command


- There are no `options` or `selection_criteria`

- The action is `{print}` enclosed in curly braces. The `print` pattern-action statement in awk tells it to print each line of the file.

- There is no `output-file`, so the result is displayed on the terminal.

Running this command will display all lines of the `data.txt` file, mirroring the functionality of the `cat` command.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Field Numbers

In `awk`, field numbers are used to refer to specific columns in a line of text. Fields are denoted by a dollar sign (`$`) followed by the field number. The records (lines of text) are automatically split into fields based on a delimiter, which is a space or tab by default but can be changed using the `-F` option.

`$1` denotes the first field of a line of text, `$2` represents the second field, and so forth. `$0` refers to the whole line.

Often, we need to extract specific columns from a file. Suppose we only want to extract the "Brand" and "Model" of each line. The code is:
```bash
#!/bin/bash

# Using `awk` to print specific columns (Brand and Model)
awk '{print $1, $2}' data.txt
```
- `$1` and `$2` refer to the first and second fields (columns) of each line in the file.

- This command skips the first and fourth columns, displaying only the brand and model of each item.

The output of the command is:

```bash
Brand Model
Apple MacBook
Apple iPad
Dell XPS
Dell Inspiron
Lenovo ThinkPad
Lenovo Yoga
Apple MacBook
```
The output successfully shows only the "Brand" and "Model" columns of the text file.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Conditional Text Selection

Often, you will need to filter lines based on specific conditions. To do this, you place the condition before the `{action}` block, enclosed in single quotes `(')`. For instance, we may want to find all entries where the RAM is 64 GB or greater.

```bash
#!/bin/bash

# Using `awk` to print lines where RAM is greater than or equal to 64
awk '$3 >= 64 {print $0}' data.txt
```

- `$3 >= 64 {print $0}` instructs awk to print any line where the third field (RAM) is 64 or greater.

- `$0` represents the entire line. 

The output is:

```bash
Brand   Model     RAM
Dell    Inspiron  128
Lenovo  ThinkPad  128
Lenovo  Yoga      256
Apple   MacBook    64
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Built-in NR Variable

`NR` stands for "Number of Records" in `awk`. It is a built-in variable that keeps track of the current line number being processed in the input file. Each time `awk` reads a new line, it increments `NR` by one. This makes `NR` useful for actions based on the line number, such as skipping headers, processing specific lines, or adding line numbers to output.

#### Skip Header Line

Suppose we want to print every line, excluding the header line ("Brand Model RAM)". The header line has an `NR` value of 1. To skip this line, we use the condition `NR > 1`

```bash
#!/bin/bash

awk 'NR > 1 {print}' data.txt
```

- `NR > 1`: This condition skips the first line (header) and prints the remaining lines.

The output is:

```bash
Apple   MacBook    32
Apple   iPad       16
Dell    XPS        32
Dell    Inspiron  128
Lenovo  ThinkPad  128
Lenovo  Yoga      256
Apple   MacBook    64
```

#### Process a Specific Line

Now let's print only the 3rd line of `data.txt`.

```bash
#!/bin/bash

awk 'NR == 3 {print}' data.txt
```
The output is:

```bash
Apple   iPad       16
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Pattern Matching with 'awk'

Pattern matching is one of the core strengths of `awk`, allowing you to perform actions only on lines that match specific patterns. The syntax for pattern matching in `awk` involves enclosing regular expressions within slashes (`/pattern/`). Suppose we only want to print lines that contain "Apple":

```bash

#!/bin/bash

# Using `awk` with pattern matching
awk '/Apple/ {print}' data.txt
```

- The `/Apple/ {print}` pattern checks each line for the string "Apple."

- If a line contains "Apple," it is printed.

The output of the command is:

```bash
Apple   MacBook    32
Apple   iPad       16
Apple   MacBook    64

```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Performing Calculations: Variables and END

The `END` keyword is used to specify an action to be executed after all lines have been processed. The syntax is:

```bash
awk '{action1} END {action2}'
```
This command will perform `action1` for every line of text. After all lines have been processed, `action2` is run once.

Now let's write a command that calculates the average RAM across all entries. To do this

- We create a `sum` variable and `count` variable.

- For each line, we add the RAM value (column `$3`) to sum and increment the `count` variable by 1.

- After all lines have been processed, we print `sum/count`

```bash
#!/bin/bash

# Using `awk` to calculate the average RAM
awk 'NR>1 {sum+=$3; count++} END {print "Average RAM:", sum/count}' data.txt
```
- `NR>1` skips the header line because it does not contain a RAM value.

- `{sum+=$3; count++}` sums the values of the third field (RAM) and increments the count.

- `END {print "Average RAM:", sum/count}` executes after processing all lines, printing the calculated average RAM.

The output of the code is:

```bash
Average RAM: 93.7143
```
&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Custom Line Messages

You can customize the output format for each line as well. We can add text to our print statement by separating strings/field references with commas. Let’s create a message for each entry.

```bash
#!/bin/bash

# Using `awk` to print a custom message for each line
awk 'NR>1 {print "Brand:", $1, "- Model:", $2, "- RAM:", $3}' data.txt
``
The output of this command is:

```bash
Brand: Apple - Model: MacBook - RAM: 32
Brand: Apple - Model: iPad - RAM: 16
Brand: Dell - Model: XPS - RAM: 32
Brand: Dell - Model: Inspiron - RAM: 128
Brand: Lenovo - Model: ThinkPad - RAM: 128
Brand: Lenovo - Model: Yoga - RAM: 256
Brand: Apple - Model: MacBook - RAM: 64
```

The output is still a bit difficult to read. Let's continue to see how to use `printf` to format the output.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Table Formatting with 'awk'

The `printf` function in `awk` offers more control over the formatting of the output compared to the `print` command. The syntax for `printf` is:

```sql
awk '{printf format, item1, item2, ..., itemN}' input-file
```
The format string includes text and format specifiers that begin with `%`. Common format specifiers include:

- `%d`: Integer

- `%s:` String

- `\n`: Newline character

Modifiers can also be added to control the width and alignment:


**Minimum Field Width**: The number between `%` and the format specifier defines the minimum width of the field.

**Positive Width**: Right-justified by default. For example, `%10s` formats a string, right-aligned, with a minimum width of 10 characters.

**Negative Width**: Left-justified if prefixed with a minus sign. For example, `%-10s` formats a string, left-aligned, with a minimum width of 10 characters.

Now, let’s format our output as a neatly aligned table:

```bash
#!/bin/bash

# Using `awk` to format output as a table
awk 'BEGIN {print "Brand    Model     RAM"} NR>1 {printf "%-8s %-10s %2d\n", $1, $2, $3}' data.txt
```

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Breakdown of the Command

`BEGIN {print "Brand Model RAM"}`

- The `BEGIN` block is executed before any lines from the input file are processed.

- `{print "Brand Model RAM"}` This prints the header row "Brand Model RAM" before processing the actual data. The string contains specific spaces to align the header with the columns that will follow.

`NR > 1` ensures that the action `{printf ...}` is applied only to lines after the first one, which is the header line.

- `%-8s`: Left-align (`-`) a string (`s`) with a width of 8 characters.

- `%-10s`: Left-align (`-`) a string (`s`) with a width of 10 characters.

- `%2d`: Print an integer (`d`) with exactly 2 digits.

- `\n`: Newline character to move to the next line after printing.

- `$1`, `$2`, `$3`: These are the fields to be printed according to the format specifiers.

- `data.txt`: The input file that contains the data to be processed.

The output of the command is:

```sql
Brand    Model     RAM
Apple    MacBook    32
Apple    iPad       16
Dell     XPS        32
Dell     Inspiron   128
Lenovo   ThinkPad   128
Lenovo   Yoga       256
Apple    MacBook    64
```
Using `printf` and formating specifiers, the output of our table looks much more clean!

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


```bash
#!/bin/bash

# Create a sample data file
cat << EOF > data.txt
Brand Model RAM
Apple MacBook 32
Apple iPad 16
Dell XPS 32
Dell Inspiron 128
Lenovo ThinkPad 128
Lenovo Yoga 256
Apple MacBook 64
EOF

# Print formatted table
echo "**Computer Inventory**" > output.txt
awk 'BEGIN {print "Brand    Model     RAM"} NR>1 {printf "%-8s %-10s %d\n", $1, $2, $3}' data.txt >> output.txt
echo "" >> output.txt

# Print "Brand" and "Model" of computers with 128 GB RAM
echo "**Computers with 128 GB RAM**" >> output.txt
awk '$3 == 128  {print $1, $2}' data.txt >> output.txt
echo "" >> output.txt

# Print lines with "Apple"
echo "**Apple Products**" >> output.txt
awk '/Apple/ {print}' data.txt  >> output.txt
echo "" >> output.txt

# Print average RAM
echo "**Average RAM**" >> output.txt
awk 'NR>1 {sum+=$3; count++} END {print "Average RAM:", sum/count}' data.txt >> output.txt

cat output.txt
```

---

```bash
#!/bin/bash

# Create a sample data file
cat << EOF > products.txt
Brand Type Price
Nike Shoes 100
Adidas Jacket 120
Puma Socks 20
Nike Hat 30
Adidas Shoes 110
Puma Jacket 90
Nike Shoes 130
EOF

# TODO: Only print the Brand and Type columns
awk '{print $1, $2}' products.txt > output.txt
echo "" >> output.txt

# TODO: Print items less than 100
awk '$3 < 100 {print $0}' products.txt >> output.txt
echo "" >> output.txt

# TODO: Print rows that contain Adidas
awk '/Adidas/ {print}' products.txt >> output.txt

cat output.txt
```

```bash
#!/bin/bash

# Create a sample data file
cat << EOF > products.txt
Brand Type Price
Nike Shoes 100
Adidas Jacket 120
Puma Socks 20
Nike Hat 30
Adidas Shoes 110
Puma Jacket 90
Nike Shoes 130
EOF

echo "**Products Table**" > output.txt
awk '{print $0}' products.txt >> output.txt
echo "" >> output.txt

echo "**Average Price**" >> output.txt
awk '{sum+=$3; count++} END {print "Average Price:", sum/count}' products.txt >> output.txt

cat output.txt
```

```sql
#!/bin/bash

# TODO: Create the operations.txt file
cat << EOF > operations.txt
Operation
Powering On
Initializing BIOS
Loading Kernel
Authenticating User
EOF

# TODO: Process operations.txt and create server.logs with the required format using awk.
awk 'BEGIN {print "Starting Computer"}
NR > 1 {print "Task", NR-1 ":", $0}
END {print "Computer Startup Complete"}' operations.txt > server.logs

cat server.logs
```
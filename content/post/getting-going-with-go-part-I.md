---
title: "Getting Going With Go Part I"
date: 2020-08-19T15:02:59-07:00
tags: ["fmt", "arrays", "switch", "range", "for loops"]
categories: ["Go", "Languages"]

draft: false
---

After hearing a lot about the Go programming language and its similarity to C, I decided to finally get around to trying to learn it (I'm currently watching Brenna Martenson's course on Frontend Masters, ["Go for JavaScript Developers"](https://frontendmasters.com/courses/go-for-js-devs)).  And since the best way to ensure you've actually learned a thing is to teach a thing, I decided to start this blog.  So join me as I muddle through the early stages of learning a language.  

In this blog post, we'll cover the anatomy of a Go file, for loops, some functions imported from the "fmt" package, switches, the range keyword, the anatomy of a function, and arrays.



# Anatomy of a File

```go
package main // Every program needs a package main.  Aside from packiage main, the package name is the name of the directory the file appears in

import "fmt" // Import packages!

function main() { // Every program needs a main function
    	fmt.Println("Hello world")
}
```

Typing `go run <file>` in a terminal will execute the main function in the file.

# Loops

### For Loops

If you're coming to Go from a language like JavaScript, this syntax will loop familiar to you (minus the parentheses):
```go
for init; condition; post {
}

// If an intializer has already been declared, you can leave a semicolon for the initializer:

i := 1
for ; i <= 100; i++ {
    fmt.Println(i)
}
```

This is synonymous with JavaScript's `while` loop
```go
for condition {
}
```

Instead of creating a while loop and passing in `true`, Go has a syntax for declaring an infinite loop:

```go
for {
}
```

# fmt package

I'm going to cover some of the functions available in the `fmt` package, but this isn't an exhaustive list and you can learn more at [golang.org/pkg/fmt](https://golang.org/pkg/fmt/)

### Writing to standard output

`fmt.Print(a ...interface{}) (n int, err error)` writes to standard output.  It returns the number of bytes written and any write error that occurred.

`fmt.Println(a ...interface{}) (n int, err error)` writes to standard output, with a newline appended.  It returns two values, an int and an error.  The int is the number of bytes that were written to standard output.  The error is any write error that occurred

If you're familiar with C, the Printf function is similar to printf in the use of format specifiers to indicate the type to print when passing in variables
`fmt.Printf(format string, a ... interface{}) (n int, err error)` also writes to standard output, but does not include a newline.  Like Println, it returns the number of bytes written and any write error that occurred.


### Writing to external file

`fmt.Fprint(w io.Writer, a ...interface{})(n int, err error)` is like `fmt.Print` but instead of writing to standard output, it writes to an external file `w`.

`fmt.Fprintln(w. io.Writer, a ...interface{}) (n int, err error)` is like `fmt.Println` but instead of writing to standard output, it writes to an external file `w`.

`fmt.Fprintf(w. io.Writer, format string, a ...interface{}) (n int, err error)` is like `fmt.Printf` but instead of writing to standard output, it writes to an external file `w`.

# Types in Go

|         	| Type Options                                                       	| Example Usage                	|
|---------	|--------------------------------------------------------------------	|------------------------------	|
| INTEGER 	| int, int8, int16, int32, int64 uint, uint8, uint16, uint32, uint64 	| var age int = 21             	|
| FLOAT   	| float32, float64 complex64, complex128                             	| var gpa float64 = 4.0        	|
| STRING  	| string                                                             	| var plant string = "ficus"   	|
| BOOLEAN 	| &&, \|\|, !, <, <=, >=, ==, !=                                     	| var canDrink bool = age > 21 	|

### How do you check the type of a variable in Go?

In JavaScript, you would use `typeof variableName`.

In Go, you import the `reflect` package and use its `TypeOf(i interface{}) Type` function, which will return the reflection Type that represents the dynamic type of the variable passed in.

Example:

```go
package main

import (
    "fmt"
    "reflect"
)

func main() {
    hope := "Springs eternal"
    fmt.Println(reflect.TypeOf(hope)) // "string"
}
```

### Type Casting (Changing Type)

In C, we typecast by using the cast operator (which is just the name of a type surrounded by parentheses), like:

```c
#include <stdio.h>

int main () {
    int total = 0;
    int array[] = {2, 33, 12, 15};
    int length = sizeof array / sizeof array[0];

    for (int i = 0; i < length; i++) {
        total += array[i];
    }

    printf("Average = %f\n", total/(float) length);
    return 0;
}
```

In Go, it's a similar idea except instead of the parentheses surrounding the type you're casting to, you pass the variable to the type.

```go
package main

import (
	"fmt"
)

func main() {
	scores := []float64{2.0, 33.0, 12.0, 15.0}
	total := 0.0
	for _, val := range scores {
		total += val
	}
	fmt.Println(total / float64(len(scores)))
}
```

# Declaring Variables

```go

// These declarations can be used anywhere
var <name> <type> = <value> // Explicitly declare type and set variable to a value
var name string = "Amy"

var <name> <type> // Explicitly declare variable to be of a certain type
var name string

var <name> = <value> // Relies on Go's type inference to determine type based on the right-hand assignment
var name = "Amy"

// This syntax can only be used in a function block, and is the most commonly used form of variable declaration:
<name> := <value> // Relies on Go's type inference to determine type based on right-hand assignemnt
name := "Amy"

// Declaring multiple values
var <name1>, <name2> <type> = <value1>, <value2>
var name, cityIMiss string = "Amy", "Derby" // Have to be of same type

var <name1>, <name2> = <value1>, <value2>
var name, cityIMiss = "Amy", "Derby" // Can be different types
var name, age = "Amy", 31

var (
    <name1> <type1> = <value1>
    <name2> <type2> = <value2>
)

var (
    name string = "Amy"
    age int = 31
)
```

# Switch Statements


In JavaScript, if you want to do a specific thing if X, Y, or Z are true, you have to list them out as separate cases and stack them:

```javascript
const animal = "cat"
switch (animal) {
    case "cat":
    case "dog":
        console.log("That's a totally normal pet.");
        break;
    case "bird":
    case "fish":
    case "guinea pig":
    case "chinchilla":
    case "snake":
        console.log("It's a little weird, but okay.");
        break;
    case "turtle":
    case "prairie dog":
    case "tarantula":
        console.log("That's less than normal.");
        break;
    default:
        console.log("I have no words.");
        break;
    }
```

In Go, you can just include it in the same case.
```go
animal := "cat"
switch animal {
    case "cat", "dog":
        fmt.Println("That's a totally normal pet.");
    case "bird", "fish", "guinea pig", "chinchilla", "snake":
        fmt.Println("It's a little weird, but okay.");
    case "turtle", "prairie dog", "tarantula":
        fmt.Println("That's less than normal");
    default:
        fmt.Println("I have no words.")
}
```

Switch statements don't even need an expression, so it's possible (and dogmatic) to write if/else if/else statements as switches in Go.

```go
func unhex(c byte) byte {
    switch {
        case '0' <= c && c <= '9':
            return c - '0'
        case 'a' <= c && c <= 'f':
            return c - 'a' + 10
        case 'A' <= c && c <= 'F':
            return c - 'A' + 10
    }
    return 0
}
```

Unlike in JavaScript and C-based languages, switch statements in Go don't have automatic fallthrough.  This means there is no need for break statements at every level.  In fact, if you want to intentionally fall through from one case to the next, you need to specifically use the `fallthrough` keyword at the end of the case statement, and it only falls through one level.  Note that allowing it to fall through does not cause it to begin evaluating subsequent statements -- it merely skips the evaluation and does the action attached to that condition.

# Range

The `range` keyword is used to iterate over a slice or map.  When ranging over a slice, two values are returned for each iteration - the index and a copy of the element at that index (in the case of strings, this means the Unicode code point for the element at that index).

```go
for _, letter := range "This is a sentence" {
    fmt.Print(letter);
}
// 841041051153210511532973211510111011610111099101

// In order to get the string value (and not codepoint), type case:
	for _, letter := range "This is a sentence" {
		fmt.Print(string(letter))
	}
// This is a sentence
```

Note:  Go does not let you declare variables and then not use them.  For these occasions, you can use an underscore to tell the compiler that you have no need for that particular variable.

# Anatomy of a Function

```go

// func <function name> (<input name><input type>) <return type> {
// }
func average(a, b, c float64) float64 {
    total := a + b + c
    return total / 3
}

// You can also specify the name of the return values and create an implicit return of them by just using the return keyword at the end of a function
// func <function name> (<input name><input type>) (<return name> <return value>) {
// }

func average(a, b, c float64) (average float64) {
    total := a + b + c
    average = total / 3
    return
}

// In order to create a variadic function (variable number of arguments), you can use ... (think of it like the spread operator in JavaScript)
func average(numbers ...float64) float64 {
    // numbers now represents a collection of all of the arguments and can be iterated over
    total := 0.0
    for _, val := range numbers {
        total += val
    }
    return total / float64(len(numbers))
} 
```

# Arrays

- Arrays are values, not references
- Passing an array to a function passes a copy of the array, not a reference to it
- The size of an array is part of its type definition and must be specified on creation

To declare an array and then assign values afterward:
```go
var scores [5]float64
scores[0] = 9
scores[1] = 1.5
scores[2] = 4.5
scores[3] = 7
scores[4] = 8
```

To declare an array and assign:
```go
var scores[5] float64 = [5]float64{9, 1.5, 4.5, 7, 8}

// Since the values you pass in have the array length attached, you can use the shorthand syntax for assignment
scores := [5]float64{9, 1.5, 4.5, 7, 8}

// If you don't want to have to count how many items you're adding to the array, you can also use ... to indicate the array length
scores := [...]float64{9, 1.5, 4.5, 7, 8}
```

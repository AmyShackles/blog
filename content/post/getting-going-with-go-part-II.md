---
title: "Getting Going With Go Part II"
date: 2020-08-20T13:02:20-07:00
lastmod: 2020-08-20T13:02:20-07:00
draft: false
tags: ["fmt", "arrays", "switch", "range", "for loops"]
categories: ["Go", "Languages"]
---

For this second installment of Getting Going With Go, we're going to cover `slices` -- how to create them, how to add more elements to them, and how to retrieve the elements of them individually.  We'll also cover how what `maps` are in Go and how to create them.
<!--more--> 


# Slices

Slices in Go are an abstraction built on top of arrays.  They consist of a pointer to the array they refer to, the length of the part of the array they represent, and the maximum length of the part of the array they represent.  If you're not careful, it's easy to confuse a slice for an array because the syntax is so similar.  For example:

```go
letters := [4]string{"a", "b", "c", "d"} // Declares and initializes an array literal
letters := []string{"a", "b", "c", "d"} // Declares and initializes a slice literal
```

It's important to remember that for an array, the number of elements in the array is a part of the type definition and must be included whereas for a slice, because it is dynamically sized, the length is not a part of the definition.

The way to declare a new slice without assigning values to it is different also.

```go
// To make an array
var a [4] int
// Because the array definition includes a length, you can immediately assign values to indexes
a[0] = 1

// To make a slice
var s []byte
// If you were to try to assign a value to s[0] now, you could get a runtime panic for trying to assign a value beyond the array/slice.  In order to allow for the assigning of values to the slice, you need to use the make function
s = make([]byte, 5, 5)
```

## make

`make([]Type, length, capacity)` allocates an array and returns a slice that refers to that array.  The length argument is to set the initial length of the underlying array.  The capacity argument is an optional argument and refers to the maximum length of the array.

When you initialize a slice this way, without using the `make` function, the capacity and slice are the same.

## Slicing an array

One of the common ways a slice is created is as a slice of an array or slice (like you would in JavaScript).  The syntax for slicing in this fashion is a number, followed by a colon, followed by a number.  When the first number is omitted, it assumes the slice starts at 0.  When the second number is omitted, it slices to the end of the array.

```go
s := []string{"R", "o", "b", "e", "r", "t"}

a := s[0:3] 
fmt.Print(a)// Rob
b := s[2:]
fmt.Print(b)// bert
c := [:3] 
fmt.Print(c)// Rob
```

Additionally, in order to inspect the length and capacity of a slice, we can use the built-in `len` and `cap` functions:

```go
fmt.Print(len(s))//  6
fmt.Print(cap(s)) // 6
fmt.Print(cap(a)) // 6
fmt.Print(len(a)) // 3
fmt.Print(cap(b)) // 4
fmt.Print(len(b)) // 4
fmt.Print(cap(c)) // 6
fmt.Print(len(c)) // 3
```

It might seem confusing that the capacity of `b` is 4 while `a` and `c` both have a capacity of 6 like the original array `s`.  The reason for this is that the capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice.  Because `b` in this example starts at the 2nd index, it has two fewer elements in capacity.

## Extending a Slice

The way that arrays in languages like JavaScript work behind the scenes when they need to increase in size is to create a new array double the size of the original array and copy the contents of the original array into the new array.

We can achieve the same functionality in Go by using the `copy` function.

The other way to make a slice larger is by using the builtin `append` function, which adds elements to the end of a slice.

### copy

`func copy(dst, src []T) int` copies the contents of the src slice into the destination slice.   Copy returns the number of elements copied, which will be the minimum of len(src) and len(dst)
```go
t := make([]byte, len(s), (cap(s)+1)*2) // The +1 is in the event that the capacity is 0
copy(t, s)
s = t
```

### append

`func append(slice []Type, elems ...Type) []Type` adds elements to the end of the slice.  If there is enough capacity to accomodate the new elements appended, it reslicing to accomodate the new elements.  If there is not enough capacity, a new array is allocated.  `append` returns the updated slice, which is why it is important to store the result of append.

```go
s = []string{"a", "b", "c"}
fmt.Println(s) // [a b c]
s = append(s, "d", "e", "f")
fmt.Println(s) // [a b c d e f]
```

To append the contents of one slice to another, you can use a syntax similar to the JavaScript spread operator to pass each element individually to the `append` function:

```go
groceryList := []string{"steak", "wine", "catfood"}
addGroceries := func (groceries ...string) []string {
    updatedList := append(groceryList, groceries...)
    return updatedList
}
fmt.Println(addGroceries("eggs", "cheese")) // [steak wine catfood eggs cheese]
```
The `groceries ...string` in the arguments combined the two strings passed into the function into a slice.  Then in the append function, the `groceries...` separated them back out into a comma-separated list of values.

# Maps

Maps are key/value pairs in Go, similar to objects in JavaScript.  To declare a map, you use the map keyword followed by square brackets with the type of the key inside the brackets followed by the type of the value.

```go
var pageReferences map[int]string
```

Like what happens with a slice when you declare it without assigning values and then try to assign values to it, assigning to a map will result in a panic.  And like with a slice, in order to be able to assign values to a map, you need to use the `make` function.

```go
var pageReferences map[int]string
pageReferences = make(map[int]string)
pageReferences[1] = "Beginning of slice text"
pageReferences[20] = "Beginning of another text"
fmt.Println(pageReferences) // map[1:Beginning of slice text 20:Beginning of another text]
```

This can also be accomplished with shorthand syntax

```go
pageReferences := make(map[int]string)
pageReferences[1] = "Beginning of slice text"
pageReferences[20] = "Beginning of another text"
fmt.Println(pageReferences) // map[1:Beginning of slice text 20:Beginning of another text]
```

In order to assign the value from a map to a variable:
```go
firstPageRef := pageReferences[1];
fmt.Println(firstPageRef)
```

However, this actually returns two values -- the value of the selected key in the map and a boolean indicating whether or not that key exists in the map.

```go
if _, ok = pageReferences[1]; ok {
    fmt.Println("Key exists!")
} else {
    fmt.Println("Key does not exist.")
}
```

To delete a key from a map:
```go
pageReferences := make(map[int]string)
pageReferences[1] = "Beginning of slice text"
pageReferences[20] = "Beginning of another text"
delete (pageReferences, 1)
fmt.Println(pageReferences) // map[20:Beginning of another text]
```


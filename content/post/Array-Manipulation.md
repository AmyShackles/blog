---
title: "Array Manipulation"
date: 2020-09-20T15:53:17-07:00
lastmod: 2020-09-20T15:53:17-07:00
draft: false
keywords: []
tags: ['coding', 'arrays']
categories: ['code problem']
---

Problem Statement:
Startying with a 1-indexed array of zeros and a list of operations, for each operation add a value to each of the array elements between two given indices, inclusive.  Once all operations have been performed, return the maximum value in the array.
<!--more--> 

# Setup

The function you are writing takes two arguments.  The first argument, _n_, represents the number of elements in the array you are performing operations on.  The second argument, _queries_ is an array of operations to perform on the array.  Each element in _queries_ is an array consisting of a starting index, ending index, and the value to be added to the elements in your array between those starting and ending indices.

# Example 
```javascript
n = 12;
queries = [
// Start, end, value to add
    [2,   7,   4],
    [5,   9,   2],
    [6,   12,  8]
]
// 1   2   3   4   5   6   7   8   9   10  11  12 // Indices
  [0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0] // Starting array
  [0,  4,  4,  4,  4,  4,  4,  0,  0,  0,  0,  0] // After processing [2, 7, 4]
  [0,  4,  4,  4,  6,  6,  6,  2,  2,  0,  0,  0] // After processing [5, 9, 2]
  [0,  4,  4,  4,  6,  14, 14, 10, 10, 8,  8,  8] // After processing [6, 12, 8]

largest = 10
```

# Naive Approach (Brute Force)
- Create an array with length of n + 1
- Initialize each element in the array with 0
- Create a variable to store the maximum value encountered, initialized to 0
- Iterate through queries array, separating out a, b, k
- Loop through the array from index a through b, incrementing each element at that index by k
- If the updated value of the array at the current index is greater than the max, update max

```javascript
function arrayManipulation(n, queries) {
    let arr = new Array(n + 1).fill(0);
    let max = 0;
    queries.forEach(([a, b, k]) => {
        for (let i = a; i <= b; i++) {
            arr[i] += k;
            if (arr[i] > max) {
                max = arr[i];
            }
        }
    })
    return max
}
```

Now, this works for some input.  But think about what happens when n is a large number.  Think about what happens if queries is a large array.  In each operation in queries, you're updating 1-n elements in the array.  That's a _lot_ of operations to be performing.  So some of the tests on HackerRank for this particular problem time out if you have a function like this as a solution.  Their input is just too large to get away with using this algorithm.  Womp womp.  Sad trombone.

But you know what?  There's no shame in not knowing how to solve this problem.  It's marked as hard and it _is_ hard.  Hell, I had to look up solutions in the Discussion section to grok how one would even approach solving this problem at scale.  And then I needed to take out a piece of paper and a pen to work through a few problems to understand how the provided solution worked.  This is one of those solutions that once you understand it, it seems obvious.  And it's beautiful.  Naturally, the solutions I found were all in either C++ or Java and that's not my language of choice for code challenges, so I adapted it into JavaScript to both ensure that I understood it and to make it easier for anyone looking to solve it in JavaScript.

# Solution

```javascript
function arrayManipulation(n, queries) {
    let arr = new Array(n + 1).fill(0);
    queries.forEach(([a, b, k]) => {
        arr[a - 1] += k;
        arr[b] -= k;
    })
    let sum = 0;
    let max = 0;
    arr.forEach(val => {
        sum += val;
        max = Math.max(sum, max)
    })
    return max;
}
```

Let's go through how this works using the earlier example.  Note that the reason we're changing the value at arr[a - 1] is because the problem statement indicated that the arrays are 1-indexed, so the array indices given are going to be off by 1 since arrays in JavaScript are 0-indexed.  The reason we change arr[b] and not arr[b-1] is that the operations are meant to be from a to b inclusive and so we want to add the end point as being after the last index operated on.

```javascript

n = 12;
queries = [
// Start, end, value to add
    [2,   7,   4],
    [5,   9,   2],
    [6,   12,  8]
]
// 1   2   3   4   5   6   7   8   9   10  11  12  13// Indices (n + 1 now)
  [0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0] // Starting array
  [0,  4,  0,  0,  0,  0,  0,  -4, 0,  0,  0,  0,  0] // After processing [2, 7, 4]
  [0,  4,  0,  0,  2,  0,  0,  -4, 0,  -2, 0,  0,  0] // After processing [5, 9, 2]
  [0,  4,  0,  0,  2,  8,  0,  -4, 0,  -2, 0,  0, -8] // After processing [6, 12, 8]

sum = 0, max = 0, arr = [0,  4,  0,  0,  2,  8,  0,  -4, 0,  -2, 0,  0, -8]
sum += 0; // sum stays 0, max stays 0
sum += 4; // sum is now 4, sum is greater than max, so max becomes 4
sum += 0; // sum stays same, max stays same
sum += 0; // sum stays same, max stays same
sum += 2; // sum is now 6; sum is greater than max, so max becomes 6;
sum += 8; // sum is now 14; sum is greater than max, so max becomes 14;
sum += 0; // sum stays same, max stays same
sum += -4; // sum is 10; max is greater than sum, so max stays 14;
sum += 0; // sum stays same, max stays same
sum += -2; // sum is 8; max is greater than sum, so max stays 14;
sum += 0; // sum stays same, max stays same
sum += 0; // sum stays same, max stays same
sum += -8; // sum is 0; max is greater than sum, so stays 14;

max = 14;
```

How does this work?  Well, because we're subtracting the value of _k_ at the index following the end index, we're only adding the value of a given _k_ for the indices we should have added that _k_ to.  Because we're only changing the values in the array to mark the beginning and end of operations, we're only performing 2 updates for each query.  We've changed a variable operation with a worst case complexity of _n_ to be constant!  Not too shabby.

---
Problem taken from [HackerRank](https://www.hackerrank.com/challenges/crush/problem) 
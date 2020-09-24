---
title: "SingleNumber"
date: 2020-09-24T12:02:48-07:00
lastmod: 2020-09-24T12:02:48-07:00
draft: false
keywords: ['bitwise', 'xor']
tags: ['coding', 'arrays', 'bitwise']
categories: ['javascript', 'code problem']
---

## Problem Statement

Given a non-empty array of integers, every element appears twice except for one.  Find that single one.  Note: Your algorithm should have a linear runtime complexity.  Could you implement it without extra memory?

## Thought Process

My initial conclusion was that no, you couldn't.  Since there's not a range for the possible numbers in the array and the array isn't already sorted, you can solve in linear time if you used an object to store counts, but you need that extra storage or sorting is O(n log n).

But see, I forgot about XOR (^).  For those of you who don't play with bits, exclusive or takes two binary numbers and for pair where one number has the bit flipped and the other doesn't, the resulting number has the bit flipped.
```
  01101
^ 10110
= 11011
```

So if you have a number and XOR it by 0, you get the number.  If you have a number and XOR it by itself, you get 0.  

Now, that's helpful, but why would you care about XOR for this problem?

Let's run through an example:
```
input:  [4,1,1,2,2,4,5,1,2,1,2]
4^1=5
5^1=4
4^2=6
6^2=4
4^4=0
0^5=5
5^1=4
4^2=6
6^1=7
7^2=5
```
## Explanation/Solution

Because all of the values that appear twice will eventually cancel themselves out, you're left with that one solitary element that has no partner.  No extra storage necessary.  Linear time.

```javascript
function singleNumber(nums) {
    return nums.reduce((a, b) => a ^ b)
}
```

Problem taken from [LeetCode](https://leetcode.com/problems/single-number/) 